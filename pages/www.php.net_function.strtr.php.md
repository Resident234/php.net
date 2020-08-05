# strtr



Here&apos;s an important real-world example use-case for strtr where str_replace will not work or will introduce obscure bugs:<br><br>

```
<?php

$strTemplate = "My name is :name, not :name2.";
$strParams = [
  ':name' => 'Dave',
  'Dave' => ':name2 or :password', // a wrench in the otherwise sensible input
  ':name2' => 'Steve',
  ':pass' => '7hf2348', // sensitive data that maybe shouldn't be here
];

echo strtr($strTemplate, $strParams);
// "My name is Dave, not Steve."

echo str_replace(array_keys($strParams), array_values($strParams), $strTemplate);
// "My name is Steve or 7hf2348word, not Steve or 7hf2348word2."

?>
```
<br><br>Any time you&apos;re trying to template out a string and don&apos;t necessarily know what the replacement keys/values will be (or fully understand the implications of and control their content and order), str_replace will introduce the potential to incorrectly match your keys because it does not expand the longest keys first.<br><br>Further, str_replace will replace in previous replacements, introducing potential for unintended nested expansions.  Doing so can put the wrong data into the "sub-template" or even give users a chance to provide input that exposes data (if they get to define some of the replacement strings).<br><br>Don&apos;t support recursive expansion unless you need it and know it will be safe.  When you do support it, do so explicitly by repeating strtr calls until no more expansions are occurring or a sane iteration limit is reached, so that the results never implicitly depend on order of your replacement keys.  Also make certain that any user input will expanded in an isolated step after any sensitive data is already expanded into the output and no longer available as input.<br><br>Note: using some character(s) around your keys to designate them also reduces the possibility of unintended mangling of output, whether maliciously triggered or otherwise.  Thus the use of a colon prefix in these examples, which you can easily enforce when accepting replacement input to your templating/translation system.  

---

Since strtr (like PHP&apos;s other string functions) treats strings as a sequence of bytes, and since UTF-8 and other multibyte encodings use - by definition - more than one byte for at least some characters, the three-string form is likely to have problems. Use the associative array form to specify the mapping.<br><br>

```
<?php
// Assuming UTF-8
$str = '&#xC4;bc &#xC4;bc'; // strtr() sees this as nine bytes (including two for each &#xC4;)
echo strtr($str, '&#xC4;', 'a'); // The second argument is equivalent to the string "\xc3\x84" so "\xc3" gets replaced by "a" and the "\x84" is ignored

echo strtr($str, array('&#xC4;' => 'a')); // Works much better
?>
```
  

---

fixed "normaliza" functions written below to include Slavic Latin characters... also, it doesn&apos;t return lowercase any more (you can easily get that by applying strtolower yourself)...<br><br>also, renamed to normalize()<br><br>

```
<?php

function normalize ($string) {
    $table = array(
        '&#x160;'=>'S', '&#x161;'=>'s', '&#x110;'=>'Dj', '&#x111;'=>'dj', '&#x17D;'=>'Z', '&#x17E;'=>'z', '&#x10C;'=>'C', '&#x10D;'=>'c', '&#x106;'=>'C', '&#x107;'=>'c',
        '&#xC0;'=>'A', '&#xC1;'=>'A', '&#xC2;'=>'A', '&#xC3;'=>'A', '&#xC4;'=>'A', '&#xC5;'=>'A', '&#xC6;'=>'A', '&#xC7;'=>'C', '&#xC8;'=>'E', '&#xC9;'=>'E',
        '&#xCA;'=>'E', '&#xCB;'=>'E', '&#xCC;'=>'I', '&#xCD;'=>'I', '&#xCE;'=>'I', '&#xCF;'=>'I', '&#xD1;'=>'N', '&#xD2;'=>'O', '&#xD3;'=>'O', '&#xD4;'=>'O',
        '&#xD5;'=>'O', '&#xD6;'=>'O', '&#xD8;'=>'O', '&#xD9;'=>'U', '&#xDA;'=>'U', '&#xDB;'=>'U', '&#xDC;'=>'U', '&#xDD;'=>'Y', '&#xDE;'=>'B', '&#xDF;'=>'Ss',
        '&#xE0;'=>'a', '&#xE1;'=>'a', '&#xE2;'=>'a', '&#xE3;'=>'a', '&#xE4;'=>'a', '&#xE5;'=>'a', '&#xE6;'=>'a', '&#xE7;'=>'c', '&#xE8;'=>'e', '&#xE9;'=>'e',
        '&#xEA;'=>'e', '&#xEB;'=>'e', '&#xEC;'=>'i', '&#xED;'=>'i', '&#xEE;'=>'i', '&#xEF;'=>'i', '&#xF0;'=>'o', '&#xF1;'=>'n', '&#xF2;'=>'o', '&#xF3;'=>'o',
        '&#xF4;'=>'o', '&#xF5;'=>'o', '&#xF6;'=>'o', '&#xF8;'=>'o', '&#xF9;'=>'u', '&#xFA;'=>'u', '&#xFB;'=>'u', '&#xFD;'=>'y', '&#xFD;'=>'y', '&#xFE;'=>'b',
        '&#xFF;'=>'y', '&#x154;'=>'R', '&#x155;'=>'r',
    );
    
    return strtr($string, $table);
}

?>
```
  

---

OK, I debugged the function (had some errors)<br>Here it is:<br><br>if(!function_exists("stritr")){<br>    function stritr($string, $one = NULL, $two = NULL){<br>/*<br>stritr - case insensitive version of strtr<br>Author: Alexander Peev<br>Posted in PHP.NET<br>*/<br>        if(  is_string( $one )  ){<br>            $two = strval( $two );<br>            $one = substr(  $one, 0, min( strlen($one), strlen($two) )  );<br>            $two = substr(  $two, 0, min( strlen($one), strlen($two) )  );<br>            $product = strtr(  $string, ( strtoupper($one) . strtolower($one) ), ( $two . $two )  );<br>            return $product;<br>        }<br>        else if(  is_array( $one )  ){<br>            $pos1 = 0;<br>            $product = $string;<br>            while(  count( $one ) &gt; 0  ){<br>                $positions = array();<br>                foreach(  $one as $from =&gt; $to  ){<br>                    if(   (  $pos2 = stripos( $product, $from, $pos1 )  ) === FALSE   ){<br>                        unset(  $one[ $from ]  );<br>                    }<br>                    else{<br>                        $positions[ $from ] = $pos2;<br>                    }<br>                }<br>                if(  count( $one ) &lt;= 0  )break;<br>                $winner = min( $positions );<br>                $key = array_search(  $winner, $positions  );<br>                $product = (   substr(  $product, 0, $winner  ) . $one[$key] . substr(  $product, ( $winner + strlen($key) )  )   );<br>                $pos1 = (  $winner + strlen( $one[$key] )  );<br>            }<br>            return $product;<br>        }<br>        else{<br>            return $string;<br>        }<br>    }/* endfunction stritr */<br>}/* endfunction exists stritr */  

---

[Official documentation page](https://www.php.net/manual/en/function.strtr.php)

**[To root](/README.md)**