# mb_convert_encoding





For my last project I needed to convert several CSV files from Windows-1250 to UTF-8, and after several days of searching around I found a function that is partially solved my problem, but it still has not transformed all the characters. So I made &#x200B;&#x200B;this:

function w1250_to_utf8($text) {
&#xA0; &#xA0; // map based on:
&#xA0; &#xA0; // http://konfiguracja.c0.pl/iso02vscp1250en.html
&#xA0; &#xA0; // http://konfiguracja.c0.pl/webpl/index_en.html#examp
&#xA0; &#xA0; // http://www.htmlentities.com/html/entities/
&#xA0; &#xA0; $map = array(
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8A) =&gt; chr(0xA9),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8C) =&gt; chr(0xA6),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8D) =&gt; chr(0xAB),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8E) =&gt; chr(0xAE),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8F) =&gt; chr(0xAC),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9C) =&gt; chr(0xB6),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9D) =&gt; chr(0xBB),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA1) =&gt; chr(0xB7),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA5) =&gt; chr(0xA1),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xBC) =&gt; chr(0xA5),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9F) =&gt; chr(0xBC),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB9) =&gt; chr(0xB1),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9A) =&gt; chr(0xB9),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xBE) =&gt; chr(0xB5),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9E) =&gt; chr(0xBE),
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x80) =&gt; &apos;&amp;euro;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x82) =&gt; &apos;&amp;sbquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x84) =&gt; &apos;&amp;bdquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x85) =&gt; &apos;&amp;hellip;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x86) =&gt; &apos;&amp;dagger;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x87) =&gt; &apos;&amp;Dagger;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x89) =&gt; &apos;&amp;permil;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8B) =&gt; &apos;&amp;lsaquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x91) =&gt; &apos;&amp;lsquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x92) =&gt; &apos;&amp;rsquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x93) =&gt; &apos;&amp;ldquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x94) =&gt; &apos;&amp;rdquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x95) =&gt; &apos;&amp;bull;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x96) =&gt; &apos;&amp;ndash;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x97) =&gt; &apos;&amp;mdash;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x99) =&gt; &apos;&amp;trade;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9B) =&gt; &apos;&amp;rsquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA6) =&gt; &apos;&amp;brvbar;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA9) =&gt; &apos;&amp;copy;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xAB) =&gt; &apos;&amp;laquo;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xAE) =&gt; &apos;&amp;reg;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB1) =&gt; &apos;&amp;plusmn;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB5) =&gt; &apos;&amp;micro;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB6) =&gt; &apos;&amp;para;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB7) =&gt; &apos;&amp;middot;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; chr(0xBB) =&gt; &apos;&amp;raquo;&apos;,
&#xA0; &#xA0; );
&#xA0; &#xA0; return html_entity_decode(mb_convert_encoding(strtr($text, $map), &apos;UTF-8&apos;, &apos;ISO-8859-2&apos;), ENT_QUOTES, &apos;UTF-8&apos;);
}

  

#



I&apos;ve been trying to find the charset of a norwegian (with a lot of &#xF8;, &#xE6;, &#xE5;) txt file written on a Mac, i&apos;ve found it in this way:





```
<?php

$text = &quot;A strange string to pass, maybe with some &#xF8;, &#xE6;, &#xE5; characters.&quot;;



foreach(mb_list_encodings() as $chr){

&#xA0; &#xA0; &#xA0; &#xA0; echo mb_convert_encoding($text, &apos;UTF-8&apos;, $chr).&quot; : &quot;.$chr.&quot;&lt;br&gt;&quot;;&#xA0; &#xA0; 

 } 

?>
```




The line that looks good, gives you the encoding it was written in.



Hope can help someone

  

#



many people below talk about using 



```
<?php

&#xA0; &#xA0; mb_convert_encode($s,&apos;HTML-ENTITIES&apos;,&apos;UTF-8&apos;);

?>
```


to convert non-ascii code into html-readable stuff.&#xA0; Due to my webserver being out of my control, I was unable to set the database character set, and whenever PHP made a copy of my $s variable that it had pulled out of the database, it would convert it to nasty latin1 automatically and not leave it in it&apos;s beautiful UTF-8 glory.



So [insert korean characters here] turned into ?????.



I found myself needing to pass by reference (which of course is deprecated/nonexistent in recent versions of PHP)

so instead of



```
<?php

&#xA0; &#xA0; mb_convert_encode(&amp;$s,&apos;HTML-ENTITIES&apos;,&apos;UTF-8&apos;);

?>
```


which worked perfectly until I upgraded, so I had to use



```
<?php

&#xA0; &#xA0; call_user_func_array(&apos;mb_convert_encoding&apos;, array(&amp;$s,&apos;HTML-ENTITIES&apos;,&apos;UTF-8&apos;));

?>
```




Hope it helps someone else out

  

#



Hey guys. For everybody who&apos;s looking for a function that is converting an iso-string to utf8 or an utf8-string to iso, here&apos;s your solution:

public function encodeToUtf8($string) {
&#xA0; &#xA0;&#xA0; return mb_convert_encoding($string, &quot;UTF-8&quot;, mb_detect_encoding($string, &quot;UTF-8, ISO-8859-1, ISO-8859-15&quot;, true));
}

public function encodeToIso($string) {
&#xA0; &#xA0;&#xA0; return mb_convert_encoding($string, &quot;ISO-8859-1&quot;, mb_detect_encoding($string, &quot;UTF-8, ISO-8859-1, ISO-8859-15&quot;, true));
}

For me these functions are working fine. Give it a try

  

#



aaron, to discard unsupported characters instead of printing a ?, you might as well simply set the configuration directive:

mbstring.substitute_character = &quot;none&quot;

in your php.ini. Be sure to include the quotes around none. Or at run-time with



```
<?php
ini_set(&apos;mbstring.substitute_character&apos;, &quot;none&quot;);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-convert-encoding.php)

**[To root](/README.md)**