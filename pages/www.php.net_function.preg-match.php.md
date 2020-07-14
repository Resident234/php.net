# preg_match



Simple regex<br><br>Regex quick reference<br>[abc]     A single character: a, b or c<br>[^abc]     Any single character but a, b, or c<br>[a-z]     Any single character in the range a-z<br>[a-zA-Z]     Any single character in the range a-z or A-Z<br>^     Start of line<br>$     End of line<br>\A     Start of string<br>\z     End of string<br>.     Any single character<br>\s     Any whitespace character<br>\S     Any non-whitespace character<br>\d     Any digit<br>\D     Any non-digit<br>\w     Any word character (letter, number, underscore)<br>\W     Any non-word character<br>\b     Any word boundary character<br>(...)     Capture everything enclosed<br>(a|b)     a or b<br>a?     Zero or one of a<br>a*     Zero or more of a<br>a+     One or more of a<br>a{3}     Exactly 3 of a<br>a{3,}     3 or more of a<br>a{3,6}     Between 3 and 6 of a<br><br>options: i case insensitive m make dot match newlines x ignore whitespace in regex o perform #{...} substitutions only once  

#

Was working on a site that needed japanese and alphabetic letters and needed to <br>validate input using preg_match, I tried using \p{script} but didn&apos;t work:<br><br>

```
<?php
$pattern =&apos;/^([-a-zA-Z0-9_\p{Katakana}\p{Hiragana}\p{Han}]*)$/u&apos;; // Didn&apos;t work
?>
```


So I tried with ranges and it worked:



```
<?php
$pattern =&apos;/^[-a-zA-Z0-9_\x{30A0}-\x{30FF}&apos;
         .&apos;\x{3040}-\x{309F}\x{4E00}-\x{9FBF}\s]*$/u&apos;;
$match_string = &apos;&#x5370;&#x5237;&#x6700;&#x5B89; &#x30CB;&#x30AD;&#x30D3;&#x8DE1;&#x9664;&#x53BB; &#x30B2;&#x30FC;&#x30E0;&#x30DC;&#x30FC;&#x30A4;&apos;;

if (preg_match($pattern, $match_string)) {
    echo "Found - pattern $pattern";
} else {
    echo "Not found - pattern $pattern";
}
?>
```
<br><br>U+4E00&#x2013;U+9FBF Kanji<br>U+3040&#x2013;U+309F Hiragana<br>U+30A0&#x2013;U+30FF Katakana<br><br>Hope its useful, it took me several hours to figure it out.  

#

I noticed that in order to deal with UTF-8 texts, without having to recompile php with the PCRE UTF-8 flag enabled, you can just add the following sequence at the start of your pattern: (*UTF8)<br><br>for instance : &apos;#(*UTF8)[[:alnum:]]#&apos; will return TRUE for &apos;&#xE9;&apos; where &apos;#[[:alnum:]]#&apos; will return FALSE<br><br>found this very very useful tip after hours of research over the web directly in pcre website right here : http://www.pcre.org/pcre.txt<br>there are many further informations about UTF-8 support in the lib<br><br>hop that will help!<br><br>--<br>cedric  

#

Sometimes its useful to negate a string. The first method which comes to mind to do this is: [^(string)] but this of course won&apos;t work. There is a solution, but it is not very well known. This is the simple piece of code on how a negation of a string is done:<br><br>(?:(?!string).)<br><br>?: makes a subpattern (see http://www.php.net/manual/en/regexp.reference.subpatterns.php) and ?! is a negative look ahead. You put the negative look ahead in front of the dot because you want the regex engine to first check if there is an occurrence of the string you are negating. Only if it is not there, you want to match an arbitrary character.<br><br>Hope this helps some ppl.  

#

This sample regexp may be useful if you are working with DB field types. <br><br>(?P&lt;type&gt;\w+)($|\((?P&lt;length&gt;(\d+|(.*)))\))<br><br>For example, if you are have a such type as "varchar(255)" or "text", the next fragment<br><br>

```
<?php
   $type = &apos;varchar(255)&apos;;  // type of field
   preg_match(&apos;/(?P&lt;type&gt;\w+)($|\((?P&lt;length&gt;(\d+|(.*)))\))/&apos;, $type, $field);
   print_r($field);
?>
```
<br><br>will output something like this:<br>Array ( [0] =&gt; varchar(255) [type] =&gt; varchar [1] =&gt; varchar [2] =&gt; (255) [length] =&gt; 255 [3] =&gt; 255 [4] =&gt; 255 )  

#

I just learned about named groups from a Python friend today and was curious if PHP supported them, guess what -- it does!!!<br><br>http://www.regular-expressions.info/named.html<br><br>

```
<?php
   preg_match("/(?P&lt;foo&gt;abc)(.*)(?P&lt;bar&gt;xyz)/",
                       &apos;abcdefghijklmnopqrstuvwxyz&apos;,
                       $matches);
   print_r($matches);
?>
```
<br><br>will produce: <br><br>Array<br>(<br>    [0] =&gt; abcdefghijklmnopqrstuvwxyz<br>    [foo] =&gt; abc<br>    [1] =&gt; abc<br>    [2] =&gt; defghijklmnopqrstuvw<br>    [bar] =&gt; xyz<br>    [3] =&gt; xyz<br>)<br><br>Note that you actually get the named group as well as the numerical key<br>value too, so if you do use them, and you&apos;re counting array elements, be<br>aware that your array might be bigger than you initially expect it to be.  

#

For those who search for a unicode regular expression example using preg_match here it is:<br><br>Check for Persian digits<br>preg_match( "/[^\x{06F0}-\x{06F9}\x]+/u" , &apos;&#x6F1;&#x6F2;&#x6F3;&#x6F4;&#x6F5;&#x6F6;&#x6F7;&#x6F8;&#x6F9;&#x6F0;&apos; );  

#

Matching a backslash character can be confusing, because double escaping is needed in the pattern: first for PHP, second for the regex engine<br>

```
<?php
//match newline control character:
preg_match(&apos;/\n/&apos;,&apos;\n&apos;);   //pattern matches and is stored as control character 0x0A in the pattern string
preg_match(&apos;/\\\n/&apos;,&apos;\n&apos;); //very same match, but is stored escaped as 0x5C,0x6E in the pattern string

//trying to match "\&apos;" (2 characters) in a text file, &apos;\\\&apos;&apos; as PHP string:
$subject = file_get_contents(&apos;myfile.txt&apos;);
preg_match(&apos;/\\\&apos;/&apos;,$subject);    //DOESN&apos;T MATCH!!! stored as 0x5C,0x27 (escaped apostrophe), this only matches apostrophe
preg_match(&apos;/\\\\\&apos;/&apos;,$subject);  //matches, stored as 0x5C,0x5C,0x27 (escaped backslash and unescaped apostrophe)
preg_match(&apos;/\\\\\\\/&apos;,$subject); //also matches, stored as 0x5C,0x5C,0x5C,0x27 (escaped backslash and escaped apostrophe)

//matching "\n" (2 characters):
preg_match(&apos;/\\\\n/&apos;,&apos;\\n&apos;);
preg_match(&apos;/\\\n/&apos;,&apos;\\n&apos;); //same match - 3 backslashes are interpreted as 2 in PHP, if the following character is not escapeable
?>
```
  

#

There does not seem to be any mention of the PHP version of switches that can be used with regular expressions.<br><br>preg_match_all(&apos;/regular expr/sim&apos;,$text).<br><br>The s i m being the location for and available switches (I know about)<br>The i is to ignore letter cases (this is commonly known - I think)<br>The s tells the code NOT TO stop searching when it encounters \n (line break) - this is important with multi-line entries for example text from an editor that needs search.<br>The m tells the code it is a multi-line entry, but importantly allows the use of ^ and $ to work when showing start and end.<br><br>I am hoping this will save someone from the 4 hours of torture that I endured, trying to workout this issue.  

#

Bugs of preg_match (PHP-version 5.2.5)<br><br>In most cases, the following example will show one of two PHP-bugs discovered with preg_match depending on your PHP-version and configuration.<br><br>

```
<?php

$text = "test=";
// creates a rather long text
for ($i = 0; $i++ &lt; 100000;)
    $text .= "%AB";

// a typical URL_query validity-checker (the pattern&apos;s function does not matter for this example)
$pattern    = &apos;/^(?:[;\/?:@&amp;=+$,]|(?:[^\W_]|[-_.!~*\()\[\] ])|(?:%[\da-fA-F]{2}))*$/&apos;;
    
var_dump( preg_match( $pattern, $text ) );

?>
```


Possible bug (1):
=============
On one of our Linux-Servers the above example crashes PHP-execution with a C(?) Segmentation Fault(!). This seems to be a known bug (see http://bugs.php.net/bug.php?id=40909), but I don&apos;t know if it has been fixed, yet.
If you are looking for a work-around, the following code-snippet is what I found helpful. It wraps the possibly crashing preg_match call by decreasing the PCRE recursion limit in order to result in a Reg-Exp error instead of a PHP-crash.



```
<?php
[...]

// decrease the PCRE recursion limit for the (possibly dangerous) preg_match call
$former_recursion_limit = ini_set( "pcre.recursion_limit", 10000 );

// the wrapped preg_match call
$result = preg_match( $pattern, $text );

// reset the PCRE recursion limit to its original value
ini_set( "pcre.recursion_limit", $former_recursion_limit );

// if the reg-exp fails due to the decreased recursion limit we may not make any statement, but PHP-execution continues
if ( PREG_RECURSION_LIMIT_ERROR === preg_last_error() )
{
    // react on the failed regular expression here
    $result = [...];
    
    // do logging or email-sending here
    [...]
} //if

?>
```
<br><br>Possible bug (2):<br>=============<br>On one of our Windows-Servers the above example does not crash PHP, but (directly) hits the recursion-limit. Here, the problem is that preg_match does not return boolean(false) as expected by the description / manual of above.<br>In short, preg_match seems to return an int(0) instead of the expected boolean(false) if the regular expression could not be executed due to the PCRE recursion-limit. So, if preg_match results in int(0) you seem to have to check preg_last_error() if maybe an error occurred.  

#

This sample is for checking persian character:<br><br>

```
<?php
   preg_match("/[\x{0600}-\x{06FF}\x]{1,32}/u", &apos;&#x645;&#x62D;&#x645;&#x62F;&apos;);
?>
```
  

#

Because making a truly correct email validation function is harder than one may think, consider using this one which comes with PHP through the filter_var function (http://www.php.net/manual/en/function.filter-var.php):<br><br>

```
<?php
$email = "someone@domain .local";

if(!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    echo "E-mail is not valid";
} else {
    echo "E-mail is valid";
}
?>
```
  

#

To validate directorys on Windows i used this:<br><br>if( preg_match("#^([a-z]{1}\:{1})?[\\\/]?([\-\w]+[\\\/]?)*$#i",$_GET[&apos;path&apos;],$matches) !== 1 ){<br>    echo("Invalid value");<br>}else{<br>    echo("Valid value");<br>}<br><br>The parts are:<br><br>#^ and $i            Make the string matches at all the pattern, from start to end for ensure a complete match.<br>([a-z]{1}\:{1})?        The string may starts with one letter and a colon, but only 1 character for eachone, this is for the drive letter (C:)<br>[\\\/]?            The string may contain, but not require 1 slash or backslash after the drive letter, (\/)<br>([\-\w]+[\\\/]?)*    The string must have 1 or more of any character like hyphen, letter, number, underscore, and may contain a slash or back slash at the end, to have a directory like ("/" or "folderName" or "folderName/"), this may be repeated one or more times.  

#

If you want to validate an email in one line, use filter_var() function !<br>http://fr.php.net/manual/en/function.filter-var.php<br><br>easy use, as described in the document example :<br>var_dump(filter_var(&apos;bob@example.com&apos;, FILTER_VALIDATE_EMAIL));  

#

As I wasted lots of time finding a REAL regex for URLs and resulted in building it on my own, I now have found one, that seems to work for all kinds of urls:<br><br>

```
<?php
    $regex = "((https?|ftp)\:\/\/)?"; // SCHEME
    $regex .= "([a-z0-9+!*(),;?&amp;=\$_.-]+(\:[a-z0-9+!*(),;?&amp;=\$_.-]+)?@)?"; // User and Pass
    $regex .= "([a-z0-9-.]*)\.([a-z]{2,3})"; // Host or IP
    $regex .= "(\:[0-9]{2,5})?"; // Port
    $regex .= "(\/([a-z0-9+\$_-]\.?)+)*\/?"; // Path
    $regex .= "(\?[a-z+&amp;\$_.-][a-z0-9;:@&amp;%=+\/\$_.-]*)?"; // GET Query
    $regex .= "(#[a-z_.-][a-z0-9+\$_.-]*)?"; // Anchor
?>
```


Then, the correct way to check against the regex ist as follows:



```
<?php
       if(preg_match("/^$regex$/", $url))
       {
               return true;
       }
?>
```
  

#

I see a lot of people trying to put together phone regex&apos;s and struggling (hey, no worries...they&apos;re complicated). Here&apos;s one that we use that&apos;s pretty nifty. It&apos;s not perfect, but it should work for most non-idealists.<br><br>*** Note: Only matches U.S. phone numbers. ***<br><br>

```
<?php

// all on one line...
$regex = &apos;/^(?:1(?:[. -])?)?(?:\((?=\d{3}\)))?([2-9]\d{2})(?:(?&lt;=\(\d{3})\))? ?(?:(?&lt;=\d{3})[.-])?([2-9]\d{2})[. -]?(\d{4})(?: (?i:ext)\.? ?(\d{1,5}))?$/&apos;;

// or broken up
$regex = &apos;/^(?:1(?:[. -])?)?(?:\((?=\d{3}\)))?([2-9]\d{2})&apos;
        .&apos;(?:(?&lt;=\(\d{3})\))? ?(?:(?&lt;=\d{3})[.-])?([2-9]\d{2})&apos;
        .&apos;[. -]?(\d{4})(?: (?i:ext)\.? ?(\d{1,5}))?$/&apos;;

?>
```


If you&apos;re wondering why all the non-capturing subpatterns (which look like this "(?:", it&apos;s so that we can do this:



```
<?php

$formatted = preg_replace($regex, &apos;($1) $2-$3 ext. $4&apos;, $phoneNumber);

// or, provided you use the $matches argument in preg_match

$formatted = "($matches[1]) $matches[2]-$matches[3]";
if ($matches[4]) $formatted .= " $matches[4]";

?>
```
<br><br>*** Results: ***<br>520-555-5542 :: MATCH<br>520.555.5542 :: MATCH<br>5205555542 :: MATCH<br>520 555 5542 :: MATCH<br>520) 555-5542 :: FAIL<br>(520 555-5542 :: FAIL<br>(520)555-5542 :: MATCH<br>(520) 555-5542 :: MATCH<br>(520) 555 5542 :: MATCH<br>520-555.5542 :: MATCH<br>520 555-0555 :: MATCH<br>(520)5555542 :: MATCH<br>520.555-4523 :: MATCH<br>19991114444 :: FAIL<br>19995554444 :: MATCH<br>514 555 1231 :: MATCH<br>1 555 555 5555 :: MATCH<br>1.555.555.5555 :: MATCH<br>1-555-555-5555 :: MATCH<br>520-555-5542 ext.123 :: MATCH<br>520.555.5542 EXT 123 :: MATCH<br>5205555542 Ext. 7712 :: MATCH<br>520 555 5542 ext 5 :: MATCH<br>520) 555-5542 :: FAIL<br>(520 555-5542 :: FAIL<br>(520)555-5542 ext .4 :: FAIL<br>(512) 555-1234 ext. 123 :: MATCH<br>1(555)555-5555 :: MATCH  

#

Some times a Hacker use a php file or shell as a image to hack your website. so if you try to use move_uploaded_file() function as in example to allow for users to upload files, you must check if this file contains a bad codes or not so we use this function. preg match<br><br>in this function we use<br><br>unlink() - http://php.net/unlink<br><br>after you upload file check a file with below function. <br><br>

```
<?php

/**
 * A simple function to check file from bad codes.
 *
 * @param (string) $file - file path.
 * @author Yousef Ismaeil - Cliprz[at]gmail[dot]com.
 */
function is_clean_file ($file)
{
    if (file_exists($file))
    {
        $contents = file_get_contents($file);
    }
    else
    {
        exit($file." Not exists.");
    }

    if (preg_match(&apos;/(base64_|eval|system|shell_|exec|php_)/i&apos;,$contents))
    {
        return true;
    }
    else if (preg_match("#&amp;\#x([0-9a-f]+);#i", $contents))
    {
        return true;
    }
    elseif (preg_match(&apos;#&amp;\#([0-9]+);#i&apos;, $contents))
    {
        return true;
    }
    elseif (preg_match("#([a-z]*)=([\`\&apos;\"]*)script:#iU", $contents))
    {
        return true;
    }
    elseif (preg_match("#([a-z]*)=([\`\&apos;\"]*)javascript:#iU", $contents))
    {
        return true;
    }
    elseif (preg_match("#([a-z]*)=([\&apos;\"]*)vbscript:#iU", $contents))
    {
        return true;
    }
    elseif (preg_match("#(&lt;[^&gt;]+)style=([\`\&apos;\"]*).*expression\([^&gt;]*&gt;#iU", $contents))
    {
        return true;
    }
    elseif (preg_match("#(&lt;[^&gt;]+)style=([\`\&apos;\"]*).*behaviour\([^&gt;]*&gt;#iU", $contents))
    {
        return true;
    }
    elseif (preg_match("#&lt;/*(applet|link|style|script|iframe|frame|frameset|html|body|title|div|p|form)[^&gt;]*&gt;#i", $contents))
    {
        return true;
    }
    else
    {
        return false;
    }
}
?>
```


Use



```
<?php
// If image contains a bad codes
$image   = "simpleimage.png";

if (is_clean_file($image))
{
    echo "Bad codes this is not image";
    unlink($image);
}
else
{
    echo "This is a real image.";
}
?>
```
  

#

for those coming over from ereg, preg_match can be quite intimidating. to get started here is a migration tip.<br><br>

```
<?php
if(ereg(&apos;[^0-9A-Za-z]&apos;,$test_string)) // will be true if characters arnt 0-9, A-Z or a-z.

if(preg_match(&apos;/[^0-9A-Za-z]/&apos;,$test_string)) // this is the preg_match version. the /&apos;s are now required.
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-match.php)

**[To root](/README.md)**