# strtr



Here&apos;s an important real-world example use-case for strtr where str_replace will not work or will introduce obscure bugs:<br><br>

```
<?php

$strTemplate = "My name is :name, not :name2.";
$strParams = [
  &apos;:name&apos; =&gt; &apos;Dave&apos;,
  &apos;Dave&apos; =&gt; &apos;:name2 or :password&apos;, // a wrench in the otherwise sensible input
  &apos;:name2&apos; =&gt; &apos;Steve&apos;,
  &apos;:pass&apos; =&gt; &apos;7hf2348&apos;, // sensitive data that maybe shouldn&apos;t be here
];

echo strtr($strTemplate, $strParams);
// "My name is Dave, not Steve."

echo str_replace(array_keys($strParams), array_values($strParams), $strTemplate);
// "My name is Steve or 7hf2348word, not Steve or 7hf2348word2."

?>
```
<br><br>Any time you&apos;re trying to template out a string and don&apos;t necessarily know what the replacement keys/values will be (or fully understand the implications of and control their content and order), str_replace will introduce the potential to incorrectly match your keys because it does not expand the longest keys first.<br><br>Further, str_replace will replace in previous replacements, introducing potential for unintended nested expansions.  Doing so can put the wrong data into the "sub-template" or even give users a chance to provide input that exposes data (if they get to define some of the replacement strings).<br><br>Don&apos;t support recursive expansion unless you need it and know it will be safe.  When you do support it, do so explicitly by repeating strtr calls until no more expansions are occurring or a sane iteration limit is reached, so that the results never implicitly depend on order of your replacement keys.  Also make certain that any user input will expanded in an isolated step after any sensitive data is already expanded into the output and no longer available as input.<br><br>Note: using some character(s) around your keys to designate them also reduces the possibility of unintended mangling of output, whether maliciously triggered or otherwise.  Thus the use of a colon prefix in these examples, which you can easily enforce when accepting replacement input to your templating/translation system.  

#

Since strtr (like PHP&apos;s other string functions) treats strings as a sequence of bytes, and since UTF-8 and other multibyte encodings use - by definition - more than one byte for at least some characters, the three-string form is likely to have problems. Use the associative array form to specify the mapping.<br><br>

```
<?php
// Assuming UTF-8
$str = &apos;&#xC4;bc &#xC4;bc&apos;; // strtr() sees this as nine bytes (including two for each &#xC4;)
echo strtr($str, &apos;&#xC4;&apos;, &apos;a&apos;); // The second argument is equivalent to the string "\xc3\x84" so "\xc3" gets replaced by "a" and the "\x84" is ignored

echo strtr($str, array(&apos;&#xC4;&apos; =&gt; &apos;a&apos;)); // Works much better
?>
```
  

#

fixed "normaliza" functions written below to include Slavic Latin characters... also, it doesn&apos;t return lowercase any more (you can easily get that by applying strtolower yourself)...<br><br>also, renamed to normalize()<br><br>

```
<?php

function normalize ($string) {
    $table = array(
        &apos;&#x160;&apos;=&gt;&apos;S&apos;, &apos;&#x161;&apos;=&gt;&apos;s&apos;, &apos;&#x110;&apos;=&gt;&apos;Dj&apos;, &apos;&#x111;&apos;=&gt;&apos;dj&apos;, &apos;&#x17D;&apos;=&gt;&apos;Z&apos;, &apos;&#x17E;&apos;=&gt;&apos;z&apos;, &apos;&#x10C;&apos;=&gt;&apos;C&apos;, &apos;&#x10D;&apos;=&gt;&apos;c&apos;, &apos;&#x106;&apos;=&gt;&apos;C&apos;, &apos;&#x107;&apos;=&gt;&apos;c&apos;,
        &apos;&#xC0;&apos;=&gt;&apos;A&apos;, &apos;&#xC1;&apos;=&gt;&apos;A&apos;, &apos;&#xC2;&apos;=&gt;&apos;A&apos;, &apos;&#xC3;&apos;=&gt;&apos;A&apos;, &apos;&#xC4;&apos;=&gt;&apos;A&apos;, &apos;&#xC5;&apos;=&gt;&apos;A&apos;, &apos;&#xC6;&apos;=&gt;&apos;A&apos;, &apos;&#xC7;&apos;=&gt;&apos;C&apos;, &apos;&#xC8;&apos;=&gt;&apos;E&apos;, &apos;&#xC9;&apos;=&gt;&apos;E&apos;,
        &apos;&#xCA;&apos;=&gt;&apos;E&apos;, &apos;&#xCB;&apos;=&gt;&apos;E&apos;, &apos;&#xCC;&apos;=&gt;&apos;I&apos;, &apos;&#xCD;&apos;=&gt;&apos;I&apos;, &apos;&#xCE;&apos;=&gt;&apos;I&apos;, &apos;&#xCF;&apos;=&gt;&apos;I&apos;, &apos;&#xD1;&apos;=&gt;&apos;N&apos;, &apos;&#xD2;&apos;=&gt;&apos;O&apos;, &apos;&#xD3;&apos;=&gt;&apos;O&apos;, &apos;&#xD4;&apos;=&gt;&apos;O&apos;,
        &apos;&#xD5;&apos;=&gt;&apos;O&apos;, &apos;&#xD6;&apos;=&gt;&apos;O&apos;, &apos;&#xD8;&apos;=&gt;&apos;O&apos;, &apos;&#xD9;&apos;=&gt;&apos;U&apos;, &apos;&#xDA;&apos;=&gt;&apos;U&apos;, &apos;&#xDB;&apos;=&gt;&apos;U&apos;, &apos;&#xDC;&apos;=&gt;&apos;U&apos;, &apos;&#xDD;&apos;=&gt;&apos;Y&apos;, &apos;&#xDE;&apos;=&gt;&apos;B&apos;, &apos;&#xDF;&apos;=&gt;&apos;Ss&apos;,
        &apos;&#xE0;&apos;=&gt;&apos;a&apos;, &apos;&#xE1;&apos;=&gt;&apos;a&apos;, &apos;&#xE2;&apos;=&gt;&apos;a&apos;, &apos;&#xE3;&apos;=&gt;&apos;a&apos;, &apos;&#xE4;&apos;=&gt;&apos;a&apos;, &apos;&#xE5;&apos;=&gt;&apos;a&apos;, &apos;&#xE6;&apos;=&gt;&apos;a&apos;, &apos;&#xE7;&apos;=&gt;&apos;c&apos;, &apos;&#xE8;&apos;=&gt;&apos;e&apos;, &apos;&#xE9;&apos;=&gt;&apos;e&apos;,
        &apos;&#xEA;&apos;=&gt;&apos;e&apos;, &apos;&#xEB;&apos;=&gt;&apos;e&apos;, &apos;&#xEC;&apos;=&gt;&apos;i&apos;, &apos;&#xED;&apos;=&gt;&apos;i&apos;, &apos;&#xEE;&apos;=&gt;&apos;i&apos;, &apos;&#xEF;&apos;=&gt;&apos;i&apos;, &apos;&#xF0;&apos;=&gt;&apos;o&apos;, &apos;&#xF1;&apos;=&gt;&apos;n&apos;, &apos;&#xF2;&apos;=&gt;&apos;o&apos;, &apos;&#xF3;&apos;=&gt;&apos;o&apos;,
        &apos;&#xF4;&apos;=&gt;&apos;o&apos;, &apos;&#xF5;&apos;=&gt;&apos;o&apos;, &apos;&#xF6;&apos;=&gt;&apos;o&apos;, &apos;&#xF8;&apos;=&gt;&apos;o&apos;, &apos;&#xF9;&apos;=&gt;&apos;u&apos;, &apos;&#xFA;&apos;=&gt;&apos;u&apos;, &apos;&#xFB;&apos;=&gt;&apos;u&apos;, &apos;&#xFD;&apos;=&gt;&apos;y&apos;, &apos;&#xFD;&apos;=&gt;&apos;y&apos;, &apos;&#xFE;&apos;=&gt;&apos;b&apos;,
        &apos;&#xFF;&apos;=&gt;&apos;y&apos;, &apos;&#x154;&apos;=&gt;&apos;R&apos;, &apos;&#x155;&apos;=&gt;&apos;r&apos;,
    );
    
    return strtr($string, $table);
}

?>
```
  

#

OK, I debugged the function (had some errors)<br>Here it is:<br><br>if(!function_exists("stritr")){<br>    function stritr($string, $one = NULL, $two = NULL){<br>/*<br>stritr - case insensitive version of strtr<br>Author: Alexander Peev<br>Posted in PHP.NET<br>*/<br>        if(  is_string( $one )  ){<br>            $two = strval( $two );<br>            $one = substr(  $one, 0, min( strlen($one), strlen($two) )  );<br>            $two = substr(  $two, 0, min( strlen($one), strlen($two) )  );<br>            $product = strtr(  $string, ( strtoupper($one) . strtolower($one) ), ( $two . $two )  );<br>            return $product;<br>        }<br>        else if(  is_array( $one )  ){<br>            $pos1 = 0;<br>            $product = $string;<br>            while(  count( $one ) &gt; 0  ){<br>                $positions = array();<br>                foreach(  $one as $from =&gt; $to  ){<br>                    if(   (  $pos2 = stripos( $product, $from, $pos1 )  ) === FALSE   ){<br>                        unset(  $one[ $from ]  );<br>                    }<br>                    else{<br>                        $positions[ $from ] = $pos2;<br>                    }<br>                }<br>                if(  count( $one ) &lt;= 0  )break;<br>                $winner = min( $positions );<br>                $key = array_search(  $winner, $positions  );<br>                $product = (   substr(  $product, 0, $winner  ) . $one[$key] . substr(  $product, ( $winner + strlen($key) )  )   );<br>                $pos1 = (  $winner + strlen( $one[$key] )  );<br>            }<br>            return $product;<br>        }<br>        else{<br>            return $string;<br>        }<br>    }/* endfunction stritr */<br>}/* endfunction exists stritr */  

#

[Official documentation page](https://www.php.net/manual/en/function.strtr.php)

**[To root](/README.md)**