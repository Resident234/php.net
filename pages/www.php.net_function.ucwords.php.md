# ucwords



My quick and dirty ucname (Upper Case Name) function.<br><br>

```
<?php
//FUNCTION

function ucname($string) {
    $string =ucwords(strtolower($string));

    foreach (array(&apos;-&apos;, &apos;\&apos;&apos;) as $delimiter) {
      if (strpos($string, $delimiter)!==false) {
        $string =implode($delimiter, array_map(&apos;ucfirst&apos;, explode($delimiter, $string)));
      }
    }
    return $string;
}
?>
```



```
<?php
//TEST

$names =array(
  &apos;JEAN-LUC PICARD&apos;,
  &apos;MILES O\&apos;BRIEN&apos;,
  &apos;WILLIAM RIKER&apos;,
  &apos;geordi la forge&apos;,
  &apos;bEvErly CRuSHeR&apos;
);
foreach ($names as $name) { print ucname("{$name}\n"); }

//PRINTS:
/*
Jean-Luc Picard
Miles O&apos;Brien
William Riker
Geordi La Forge
Beverly Crusher
*/
?>
```
<br><br>You can add more delimiters in the for-each loop array if you want to handle more characters.  

#

Para formatar nomes em pt-br:<br><br>

```
<?php

    function titleCase($string, $delimiters = array(" ", "-", ".", "&apos;", "O&apos;", "Mc"), $exceptions = array("de", "da", "dos", "das", "do", "I", "II", "III", "IV", "V", "VI"))
    {
        /*
         * Exceptions in lower case are words you don&apos;t want converted
         * Exceptions all in upper case are any words you don&apos;t want converted to title case
         *   but should be converted to upper case, e.g.:
         *   king henry viii or king henry Viii should be King Henry VIII
         */
        $string = mb_convert_case($string, MB_CASE_TITLE, "UTF-8");
        foreach ($delimiters as $dlnr =&gt; $delimiter) {
            $words = explode($delimiter, $string);
            $newwords = array();
            foreach ($words as $wordnr =&gt; $word) {
                if (in_array(mb_strtoupper($word, "UTF-8"), $exceptions)) {
                    // check exceptions list for any words that should be in upper case
                    $word = mb_strtoupper($word, "UTF-8");
                } elseif (in_array(mb_strtolower($word, "UTF-8"), $exceptions)) {
                    // check exceptions list for any words that should be in upper case
                    $word = mb_strtolower($word, "UTF-8");
                } elseif (!in_array($word, $exceptions)) {
                    // convert to uppercase (non-utf8 only)
                    $word = ucfirst($word);
                }
                array_push($newwords, $word);
            }
            $string = join($delimiter, $newwords);
       }//foreach
       return $string;
    }

?>
```


Usage:



```
<?php
    $s = &apos;S&#xC3;O JO&#xC3;O DOS SANTOS&apos;;
    $v = titleCase($s); // &apos;S&#xE3;o Jo&#xE3;o dos Santos&apos; 
?>
```
  

#

Some recipes for switching between underscore and camelcase naming:<br><br>

```
<?php
// underscored to upper-camelcase
// e.g. "this_method_name" -&gt; "ThisMethodName"
preg_replace(&apos;/(?:^|_)(.?)/e&apos;,"strtoupper(&apos;$1&apos;)",$string);

// underscored to lower-camelcase
// e.g. "this_method_name" -&gt; "thisMethodName"
preg_replace(&apos;/_(.?)/e&apos;,"strtoupper(&apos;$1&apos;)",$string);

// camelcase (lower or upper) to underscored
// e.g. "thisMethodName" -&gt; "this_method_name"
// e.g. "ThisMethodName" -&gt; "this_method_name"
strtolower(preg_replace(&apos;/([^A-Z])([A-Z])/&apos;, "$1_$2", $string));
?>
```
<br><br>Of course these aren&apos;t 100% symmetric.  For example...<br>  * this_is_a_string -&gt; ThisIsAString -&gt; this_is_astring<br>  * GetURLForString -&gt; get_urlfor_string -&gt; GetUrlforString  

#

Features:<br>- multi byte compatible<br>- handles multiple delimiters<br><br>

```
<?php
function ucwords_specific ($string, $delimiters = &apos;&apos;, $encoding = NULL)
{
    if ($encoding === NULL) { $encoding = mb_internal_encoding();}

    if (is_string($delimiters))
    {
        $delimiters =  str_split( str_replace(&apos; &apos;, &apos;&apos;, $delimiters));
    }

    $delimiters_pattern1 = array();
    $delimiters_replace1 = array();
    $delimiters_pattern2 = array();
    $delimiters_replace2 = array();
    foreach ($delimiters as $delimiter)
    {
        $uniqid = uniqid();
        $delimiters_pattern1[]   = &apos;/&apos;. preg_quote($delimiter) .&apos;/&apos;;
        $delimiters_replace1[]   = $delimiter.$uniqid.&apos; &apos;;
        $delimiters_pattern2[]   = &apos;/&apos;. preg_quote($delimiter.$uniqid.&apos; &apos;) .&apos;/&apos;;
        $delimiters_replace2[]   = $delimiter;
    }

    // $return_string = mb_strtolower($string, $encoding);
    $return_string = $string;
    $return_string = preg_replace($delimiters_pattern1, $delimiters_replace1, $return_string);

    $words = explode(&apos; &apos;, $return_string);

    foreach ($words as $index =&gt; $word)
    {
        $words[$index] = mb_strtoupper(mb_substr($word, 0, 1, $encoding), $encoding).mb_substr($word, 1, mb_strlen($word, $encoding), $encoding);
    }

    $return_string = implode(&apos; &apos;, $words);

    $return_string = preg_replace($delimiters_pattern2, $delimiters_replace2, $return_string);

    return $return_string;
}
?>
```


Params:
1. string: The string being converted
2. delimiters: a string with all wanted delimiters written one after the other e.g. "-&apos;"
3. encoding: Is the character encoding. If it is omitted, the internal character encoding value will be used.

Example Usage:


```
<?php
mb_internal_encoding(&apos;UTF-8&apos;);
$string = "JEAN-PAUL d&apos;artagnan &#x15F;&#x160;-&#xF2;&#xC0;-&#xE9;&#xCC; hello - world";
echo ucwords_specific( mb_strtolower($string, &apos;UTF-8&apos;), "-&apos;");
?>
```
<br><br>Output:<br>Jean-Paul D&apos;Artagnan &#x15E;&#x161;-&#xD2;&#xE0;-&#xC9;&#xEC; Hello - World  

#

[Official documentation page](https://www.php.net/manual/en/function.ucwords.php)

**[To root](/README.md)**