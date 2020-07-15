# json_decode



Make sure you pass in utf8 content, or json_decode may error out and just return a null value.  For a particular web service I was using, I had to do the following:<br><br>

```
<?php
$contents = file_get_contents($url);
$contents = utf8_encode($contents);
$results = json_decode($contents);
?>
```
<br><br>Hope this helps!  

#

First of all, since JSON is not native PHP format or even native JavaScript format, everyone who wants to use JSON wisely should carefuly read official documentation. There is a link to it here, in "Introduction" section. Many questions like "it doesn&apos;t recognize my strings" and those like previous one (about zip codes) will drop if you will be attentive!<br><br>And second. I&apos;ve found that there is no good, real working example of how to validate string if it is a JSON or not.<br>There are two ways to make this: parse input string for yourself using regular expressions or anything else, use json_decode to do it for you.<br>Parsing for yourself is like writing your own compiler, too difficult.<br>Just testing result of json_decode is not enough because you should test it with NULL, but valid JSON could be like this &apos;null&apos; and it will evaluate to NULL. So you should use another function - json_last_error. It will return error code of the last encode/decode operation. If no error occured it will be JSON_ERROR_NONE. So here is the function you should use for testing:<br>

```
<?php
function isValidJson($strJson) {
    json_decode($strJson);
    return (json_last_error() === JSON_ERROR_NONE);
}
?>
```

It's so simple, that there is no need to use it and slow down your script with extra delay for function call. Just do it manualy in you code while working with input data:


```
<?php
//here is my initial string
$sJson = $_POST['json'];
//try to decode it
$json = json_decode($sJson);
if (json_last_error() === JSON_ERROR_NONE) {
    //do something with $json. It's ready to use
} else {
    //yep, it's not JSON. Log error or alert someone or do nothing
}
?>
```
  

#

The function by 1franck to allow comments works except if there is a comment at the very beginning of the file. Here&apos;s a modified version (only the regex was changed) that accounts for that.<br><br>

```
<?php
function json_clean_decode($json, $assoc = false, $depth = 512, $options = 0) {
    // search and remove comments like /* */ and //
    $json = preg_replace("#(/\*([^*]|[\r\n]|(\*+([^*/]|[\r\n])))*\*+/)|([\s\t]//.*)|(^//.*)#", '', $json);
    
    if(version_compare(phpversion(), '5.4.0', '>=')) {
        $json = json_decode($json, $assoc, $depth, $options);
    }
    elseif(version_compare(phpversion(), '5.3.0', '>=')) {
        $json = json_decode($json, $assoc, $depth);
    }
    else {
        $json = json_decode($json, $assoc);
    }

    return $json;
}
?>
```
  

#

When decoding strings from the database, make sure the input was encoded with the correct charset when it was input to the database.<br><br>I was using a form to create records in the DB which had a content field that was valid JSON, but it included curly apostrophes.  If the page with the form did not have <br><br>&lt;meta http-equiv="Content-Type" content="text/html;charset=utf-8"&gt;<br><br>in the head, then the data was sent to the database with the wrong encoding.  Then, when json_decode tried to convert the string to an object, it failed every time.  

#

Sometime, i need to allow comments in json file. So i wrote a small func to clean comments in a json string before decoding it:<br>

```
<?php
/**
 * Clean comments of json content and decode it with json_decode().
 * Work like the original php json_decode() function with the same params
 *
 * @param   string  $json    The json string being decoded
 * @param   bool    $assoc   When TRUE, returned objects will be converted into associative arrays. 
 * @param   integer $depth   User specified recursion depth. (>=5.3)
 * @param   integer $options Bitmask of JSON decode options. (>=5.4)
 * @return  string
 */
function json_clean_decode($json, $assoc = false, $depth = 512, $options = 0) {

    // search and remove comments like /* */ and //
    $json = preg_replace("#(/\*([^*]|[\r\n]|(\*+([^*/]|[\r\n])))*\*+/)|([\s\t](//).*)#", '', $json);

    if(version_compare(phpversion(), '5.4.0', '>=')) { 
        $json = json_decode($json, $assoc, $depth, $options);
    }
    elseif(version_compare(phpversion(), '5.3.0', '>=')) { 
        $json = json_decode($json, $assoc, $depth);
    }
    else {
        $json = json_decode($json, $assoc);
    }

    return $json;
}
?>
```
  

#

it seems, that some of the people are not aware, that if you are using json_decode to decode a string it HAS to be a propper json string:<br><br>

```
<?php
var_dump(json_encode('Hello'));

var_dump(json_decode('Hello'));  // wrong
var_dump(json_decode("Hello")); // wrong
var_dump(json_decode('"Hello"')); // correct
var_dump(json_decode("'Hello'")); // wrong

result:

string(7) ""Hello""
NULL
NULL
string(5) "Hello"
NULL?>
```
  

#

If var_dump produces NULL, you may be experiencing JSONP aka JSON with padding, here&apos;s a quick fix...<br><br>

```
<?php

//remove padding
$body=preg_replace('/.+?({.+}).+/','$1',$body);

// now, process the JSON string
$result = json_decode($body);

var_dump($result);
?>
```
  

#

Be aware, when decoding JSON strings, where an empty string is a key, this library replaces the empty string with "_empty_".<br><br>So the following code gives an unexpected result:<br>

```
<?php
var_dump(json_decode('{"":"arbitrary"}'));
?>
```
<br><br>The result is as follows:<br>object(stdClass)#1 (1) {<br>  ["_empty_"]=&gt;<br>  string(6) "arbitrary"<br>}<br><br>Any subsequent key named "_empty_" (or "" [the empty string] again) will overwrite the value.  

#

I added a 3rd regex to the json_decode_nice function by "colin.mollenhour.com" to handle a trailing comma in json definition.<br><br>

```
<?php
// http://www.php.net/manual/en/function.json-decode.php#95782
function json_decode_nice($json, $assoc = FALSE){ 
    $json = str_replace(array("\n","\r"),"",$json); 
    $json = preg_replace('/([{,]+)(\s*)([^"]+?)\s*:/','$1"$3":',$json);
    $json = preg_replace('/(,)\s*}$/','}',$json);
    return json_decode($json,$assoc); 
}
?>
```


Example:



```
<?php
$dat_json = <<<EOF
{
    "foo"    : "bam",
    "bar"    : "baz",
}
EOF;
$dat_array = json_decode_nice( $dat_json );
var_dump ( $dat_json, $dat_array ); 

/* RESULTS:

string(35) "{
    "foo"    : "bam",
    "bar"    : "baz",
}"
array(2) {
  ["foo"]=>
  string(3) "bam"
  ["bar"]=>
  string(3) "baz"
}
*/
?>
```
  

#

json_decode_nice + keep linebreaks:<br><br>function json_decode_nice($json, $assoc = TRUE){<br>    $json = str_replace(array("\n","\r"),"\\n",$json);<br>    $json = preg_replace(&apos;/([{,]+)(\s*)([^"]+?)\s*:/&apos;,&apos;$1"$3":&apos;,$json);<br>    $json = preg_replace(&apos;/(,)\s*}$/&apos;,&apos;}&apos;,$json);<br>    return json_decode($json,$assoc);<br>}<br><br>by phpdoc at badassawesome dot com, I just changed line 2.<br>If you want to keep the linebreaks just escape the slash.  

#

Noted in a comment below is that this function will return NULL when given a simple string.<br><br>This is new behavior - see the result in PHP 5.2.4 :<br>php &gt; var_dump(json_decode(&apos;this is a simple string&apos;));<br>string(23) "this is a simple string"<br><br>in PHP 5.3.2 :<br>php &gt; var_dump(json_decode(&apos;this is a simple string&apos;));<br>NULL<br><br>I had several functions that relied on checking the value of a purported JSON string if it didn&apos;t decode into an object/array. If you do too, be sure to be aware of this when upgrading to PHP 5.3.  

#

This function will remove trailing commas and encode in utf8, which might solve many people&apos;s problems. Someone might want to expand it to also change single quotes to double quotes, and fix other kinds of json breakage.<br><br>

```
<?php
    function mjson_decode($json)
    {
        return json_decode(removeTrailingCommas(utf8_encode($json)));
    }
    
    function removeTrailingCommas($json)
    {
        $json=preg_replace('/,\s*([\]}])/m', '$1', $json);
        return $json;
    }
?>
```
  

#

For those of you wanting json_decode to be a little more lenient (more like Javascript), here is a wrapper:<br><br>

```
<?php
function json_decode_nice($json, $assoc = FALSE){
    $json = str_replace(array("\n","\r"),"",$json);
    $json = preg_replace('/([{,]+)(\s*)([^"]+?)\s*:/','$1"$3":',$json);
    return json_decode($json,$assoc);
}
?>
```


Some examples of accepted syntax:



```
<?php
$json = '{a:{b:"c",d:["e","f",0]}}';
$json = 
'{
   a : {
      b : "c",
      "d.e.f": "g"
   }
}';
?>
```


If your content needs to have newlines, do this:



```
<?php
$string = "This
Text
Has
Newlines";
$json = '{withnewlines:'.json_encode($string).'}';
?>
```
<br><br>Note: This does not fix trailing commas or single quotes.<br><br>[EDIT BY danbrown AT php DOT net: Contains a bugfix provided by (sskaje AT gmail DOT com) on 05-DEC-2012 with the following note.]<br><br>Old regexp failed when json like<br>{aaa:[{a:1},{a:2}]}  

#

json_decode()&apos;s handling of invalid JSON is very flaky, and it is very hard to reliably determine if the decoding succeeded or not. Observe the following examples, none of which contain valid JSON:<br><br>The following each returns NULL, as you might expect:<br><br>

```
<?php
var_dump(json_decode('['));             // unmatched bracket
var_dump(json_decode('{'));             // unmatched brace
var_dump(json_decode('{}}'));           // unmatched brace
var_dump(json_decode('{error error}')); // invalid object key/value
notation
var_dump(json_decode('["\"]'));         // unclosed string
var_dump(json_decode('[" \x "]'));      // invalid escape code

Yet the following each returns the literal string you passed to it:

var_dump(json_decode(' [')); // unmatched bracket
var_dump(json_decode(' {')); // unmatched brace
var_dump(json_decode(' {}}')); // unmatched brace
var_dump(json_decode(' {error error}')); // invalid object key/value notation
var_dump(json_decode('"\"')); // unclosed string
var_dump(json_decode('" \x "')); // invalid escape code
?>
```
<br><br>(this is on PHP 5.2.6)<br><br>Reported as a bug, but oddly enough, it was closed as not a bug.<br><br>[NOTE BY danbrown AT php DOT net: This was later re-evaluated and it was determined that an issue did in fact exist, and was patched by members of the Development Team.  See http://bugs.php.net/bug.php?id=45989 for details.]  

#

[Official documentation page](https://www.php.net/manual/en/function.json-decode.php)

**[To root](/README.md)**