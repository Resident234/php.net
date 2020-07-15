# ucwords



My quick and dirty ucname (Upper Case Name) function.<br><br>

```
<?php
//FUNCTION

function ucname($string) {
    $string =ucwords(strtolower($string));

    foreach (array('-', '\'') as $delimiter) {
      if (strpos($string, $delimiter)!==false) {
        $string =implode($delimiter, array_map('ucfirst', explode($delimiter, $string)));
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
  'JEAN-LUC PICARD',
  'MILES O\'BRIEN',
  'WILLIAM RIKER',
  'geordi la forge',
  'bEvErly CRuSHeR'
);
foreach ($names as $name) { print ucname("{$name}\n"); }

//PRINTS:
/*
Jean-Luc Picard
Miles O'Brien
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

    function titleCase($string, $delimiters = array(" ", "-", ".", "'", "O'", "Mc"), $exceptions = array("de", "da", "dos", "das", "do", "I", "II", "III", "IV", "V", "VI"))
    {
        /*
         * Exceptions in lower case are words you don't want converted
         * Exceptions all in upper case are any words you don't want converted to title case
         *   but should be converted to upper case, e.g.:
         *   king henry viii or king henry Viii should be King Henry VIII
         */
        $string = mb_convert_case($string, MB_CASE_TITLE, "UTF-8");
        foreach ($delimiters as $dlnr => $delimiter) {
            $words = explode($delimiter, $string);
            $newwords = array();
            foreach ($words as $wordnr => $word) {
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
    $s = 'S&#xC3;O JO&#xC3;O DOS SANTOS';
    $v = titleCase($s); // 'S&#xE3;o Jo&#xE3;o dos Santos' 
?>
```
  

#

Some recipes for switching between underscore and camelcase naming:<br><br>

```
<?php
// underscored to upper-camelcase
// e.g. "this_method_name" -> "ThisMethodName"
preg_replace('/(?:^|_)(.?)/e',"strtoupper('$1')",$string);

// underscored to lower-camelcase
// e.g. "this_method_name" -> "thisMethodName"
preg_replace('/_(.?)/e',"strtoupper('$1')",$string);

// camelcase (lower or upper) to underscored
// e.g. "thisMethodName" -> "this_method_name"
// e.g. "ThisMethodName" -> "this_method_name"
strtolower(preg_replace('/([^A-Z])([A-Z])/', "$1_$2", $string));
?>
```
<br><br>Of course these aren&apos;t 100% symmetric.  For example...<br>  * this_is_a_string -&gt; ThisIsAString -&gt; this_is_astring<br>  * GetURLForString -&gt; get_urlfor_string -&gt; GetUrlforString  

#

Features:<br>- multi byte compatible<br>- handles multiple delimiters<br><br>

```
<?php
function ucwords_specific ($string, $delimiters = '', $encoding = NULL)
{
    if ($encoding === NULL) { $encoding = mb_internal_encoding();}

    if (is_string($delimiters))
    {
        $delimiters =  str_split( str_replace(' ', '', $delimiters));
    }

    $delimiters_pattern1 = array();
    $delimiters_replace1 = array();
    $delimiters_pattern2 = array();
    $delimiters_replace2 = array();
    foreach ($delimiters as $delimiter)
    {
        $uniqid = uniqid();
        $delimiters_pattern1[]   = '/'. preg_quote($delimiter) .'/';
        $delimiters_replace1[]   = $delimiter.$uniqid.' ';
        $delimiters_pattern2[]   = '/'. preg_quote($delimiter.$uniqid.' ') .'/';
        $delimiters_replace2[]   = $delimiter;
    }

    // $return_string = mb_strtolower($string, $encoding);
    $return_string = $string;
    $return_string = preg_replace($delimiters_pattern1, $delimiters_replace1, $return_string);

    $words = explode(' ', $return_string);

    foreach ($words as $index => $word)
    {
        $words[$index] = mb_strtoupper(mb_substr($word, 0, 1, $encoding), $encoding).mb_substr($word, 1, mb_strlen($word, $encoding), $encoding);
    }

    $return_string = implode(' ', $words);

    $return_string = preg_replace($delimiters_pattern2, $delimiters_replace2, $return_string);

    return $return_string;
}
?>
```


Params:
1. string: The string being converted
2. delimiters: a string with all wanted delimiters written one after the other e.g. "-'"
3. encoding: Is the character encoding. If it is omitted, the internal character encoding value will be used.

Example Usage:


```
<?php
mb_internal_encoding('UTF-8');
$string = "JEAN-PAUL d'artagnan &#x15F;&#x160;-&#xF2;&#xC0;-&#xE9;&#xCC; hello - world";
echo ucwords_specific( mb_strtolower($string, 'UTF-8'), "-'");
?>
```
<br><br>Output:<br>Jean-Paul D&apos;Artagnan &#x15E;&#x161;-&#xD2;&#xE0;-&#xC9;&#xEC; Hello - World  

#

[Official documentation page](https://www.php.net/manual/en/function.ucwords.php)

**[To root](/README.md)**