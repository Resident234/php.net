# mb_convert_encoding



For my last project I needed to convert several CSV files from Windows-1250 to UTF-8, and after several days of searching around I found a function that is partially solved my problem, but it still has not transformed all the characters. So I made &#x200B;&#x200B;this:<br><br>function w1250_to_utf8($text) {<br>    // map based on:<br>    // http://konfiguracja.c0.pl/iso02vscp1250en.html<br>    // http://konfiguracja.c0.pl/webpl/index_en.html#examp<br>    // http://www.htmlentities.com/html/entities/<br>    $map = array(<br>        chr(0x8A) =&gt; chr(0xA9),<br>        chr(0x8C) =&gt; chr(0xA6),<br>        chr(0x8D) =&gt; chr(0xAB),<br>        chr(0x8E) =&gt; chr(0xAE),<br>        chr(0x8F) =&gt; chr(0xAC),<br>        chr(0x9C) =&gt; chr(0xB6),<br>        chr(0x9D) =&gt; chr(0xBB),<br>        chr(0xA1) =&gt; chr(0xB7),<br>        chr(0xA5) =&gt; chr(0xA1),<br>        chr(0xBC) =&gt; chr(0xA5),<br>        chr(0x9F) =&gt; chr(0xBC),<br>        chr(0xB9) =&gt; chr(0xB1),<br>        chr(0x9A) =&gt; chr(0xB9),<br>        chr(0xBE) =&gt; chr(0xB5),<br>        chr(0x9E) =&gt; chr(0xBE),<br>        chr(0x80) =&gt; &apos;&amp;euro;&apos;,<br>        chr(0x82) =&gt; &apos;&amp;sbquo;&apos;,<br>        chr(0x84) =&gt; &apos;&amp;bdquo;&apos;,<br>        chr(0x85) =&gt; &apos;&amp;hellip;&apos;,<br>        chr(0x86) =&gt; &apos;&amp;dagger;&apos;,<br>        chr(0x87) =&gt; &apos;&amp;Dagger;&apos;,<br>        chr(0x89) =&gt; &apos;&amp;permil;&apos;,<br>        chr(0x8B) =&gt; &apos;&amp;lsaquo;&apos;,<br>        chr(0x91) =&gt; &apos;&amp;lsquo;&apos;,<br>        chr(0x92) =&gt; &apos;&amp;rsquo;&apos;,<br>        chr(0x93) =&gt; &apos;&amp;ldquo;&apos;,<br>        chr(0x94) =&gt; &apos;&amp;rdquo;&apos;,<br>        chr(0x95) =&gt; &apos;&amp;bull;&apos;,<br>        chr(0x96) =&gt; &apos;&amp;ndash;&apos;,<br>        chr(0x97) =&gt; &apos;&amp;mdash;&apos;,<br>        chr(0x99) =&gt; &apos;&amp;trade;&apos;,<br>        chr(0x9B) =&gt; &apos;&amp;rsquo;&apos;,<br>        chr(0xA6) =&gt; &apos;&amp;brvbar;&apos;,<br>        chr(0xA9) =&gt; &apos;&amp;copy;&apos;,<br>        chr(0xAB) =&gt; &apos;&amp;laquo;&apos;,<br>        chr(0xAE) =&gt; &apos;&amp;reg;&apos;,<br>        chr(0xB1) =&gt; &apos;&amp;plusmn;&apos;,<br>        chr(0xB5) =&gt; &apos;&amp;micro;&apos;,<br>        chr(0xB6) =&gt; &apos;&amp;para;&apos;,<br>        chr(0xB7) =&gt; &apos;&amp;middot;&apos;,<br>        chr(0xBB) =&gt; &apos;&amp;raquo;&apos;,<br>    );<br>    return html_entity_decode(mb_convert_encoding(strtr($text, $map), &apos;UTF-8&apos;, &apos;ISO-8859-2&apos;), ENT_QUOTES, &apos;UTF-8&apos;);<br>}  

#

I&apos;ve been trying to find the charset of a norwegian (with a lot of &#xF8;, &#xE6;, &#xE5;) txt file written on a Mac, i&apos;ve found it in this way:<br><br>

```
<?php
$text = "A strange string to pass, maybe with some &#xF8;, &#xE6;, &#xE5; characters.";

foreach(mb_list_encodings() as $chr){
        echo mb_convert_encoding($text, 'UTF-8', $chr)." : ".$chr."&lt;br&gt;";    
 } 
?>
```
<br><br>The line that looks good, gives you the encoding it was written in.<br><br>Hope can help someone  

#

many people below talk about using <br>

```
<?php
    mb_convert_encode($s,'HTML-ENTITIES','UTF-8');
?>
```

to convert non-ascii code into html-readable stuff.  Due to my webserver being out of my control, I was unable to set the database character set, and whenever PHP made a copy of my $s variable that it had pulled out of the database, it would convert it to nasty latin1 automatically and not leave it in it's beautiful UTF-8 glory.

So [insert korean characters here] turned into ?????.

I found myself needing to pass by reference (which of course is deprecated/nonexistent in recent versions of PHP)
so instead of


```
<?php
    mb_convert_encode(&amp;$s,'HTML-ENTITIES','UTF-8');
?>
```

which worked perfectly until I upgraded, so I had to use


```
<?php
    call_user_func_array('mb_convert_encoding', array(&amp;$s,'HTML-ENTITIES','UTF-8'));
?>
```
<br><br>Hope it helps someone else out  

#

Hey guys. For everybody who&apos;s looking for a function that is converting an iso-string to utf8 or an utf8-string to iso, here&apos;s your solution:<br><br>public function encodeToUtf8($string) {<br>     return mb_convert_encoding($string, "UTF-8", mb_detect_encoding($string, "UTF-8, ISO-8859-1, ISO-8859-15", true));<br>}<br><br>public function encodeToIso($string) {<br>     return mb_convert_encoding($string, "ISO-8859-1", mb_detect_encoding($string, "UTF-8, ISO-8859-1, ISO-8859-15", true));<br>}<br><br>For me these functions are working fine. Give it a try  

#

aaron, to discard unsupported characters instead of printing a ?, you might as well simply set the configuration directive:<br><br>mbstring.substitute_character = "none"<br><br>in your php.ini. Be sure to include the quotes around none. Or at run-time with<br><br>

```
<?php
ini_set('mbstring.substitute_character', "none");
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-convert-encoding.php)

**[To root](/README.md)**