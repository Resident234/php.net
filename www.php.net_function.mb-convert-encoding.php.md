# mb_convert_encoding




<div class="phpcode"><span class="html">
For my last project I needed to convert several CSV files from Windows-1250 to UTF-8, and after several days of searching around I found a function that is partially solved my problem, but it still has not transformed all the characters. So I made &#x200B;&#x200B;this:<br><br>function w1250_to_utf8($text) {<br>&#xA0; &#xA0; // map based on:<br>&#xA0; &#xA0; // <a href="http://konfiguracja.c0.pl/iso02vscp1250en.html" rel="nofollow" target="_blank">http://konfiguracja.c0.pl/iso02vscp1250en.html</a><br>&#xA0; &#xA0; // <a href="http://konfiguracja.c0.pl/webpl/index_en.html#examp" rel="nofollow" target="_blank">http://konfiguracja.c0.pl/webpl/index_en.html#examp</a><br>&#xA0; &#xA0; // <a href="http://www.htmlentities.com/html/entities/" rel="nofollow" target="_blank">http://www.htmlentities.com/html/entities/</a><br>&#xA0; &#xA0; $map = array(<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8A) =&gt; chr(0xA9),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8C) =&gt; chr(0xA6),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8D) =&gt; chr(0xAB),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8E) =&gt; chr(0xAE),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8F) =&gt; chr(0xAC),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9C) =&gt; chr(0xB6),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9D) =&gt; chr(0xBB),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA1) =&gt; chr(0xB7),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA5) =&gt; chr(0xA1),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xBC) =&gt; chr(0xA5),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9F) =&gt; chr(0xBC),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB9) =&gt; chr(0xB1),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9A) =&gt; chr(0xB9),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xBE) =&gt; chr(0xB5),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9E) =&gt; chr(0xBE),<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x80) =&gt; &apos;&amp;euro;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x82) =&gt; &apos;&amp;sbquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x84) =&gt; &apos;&amp;bdquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x85) =&gt; &apos;&amp;hellip;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x86) =&gt; &apos;&amp;dagger;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x87) =&gt; &apos;&amp;Dagger;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x89) =&gt; &apos;&amp;permil;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x8B) =&gt; &apos;&amp;lsaquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x91) =&gt; &apos;&amp;lsquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x92) =&gt; &apos;&amp;rsquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x93) =&gt; &apos;&amp;ldquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x94) =&gt; &apos;&amp;rdquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x95) =&gt; &apos;&amp;bull;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x96) =&gt; &apos;&amp;ndash;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x97) =&gt; &apos;&amp;mdash;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x99) =&gt; &apos;&amp;trade;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0x9B) =&gt; &apos;&amp;rsquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA6) =&gt; &apos;&amp;brvbar;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xA9) =&gt; &apos;&amp;copy;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xAB) =&gt; &apos;&amp;laquo;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xAE) =&gt; &apos;&amp;reg;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB1) =&gt; &apos;&amp;plusmn;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB5) =&gt; &apos;&amp;micro;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB6) =&gt; &apos;&amp;para;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xB7) =&gt; &apos;&amp;middot;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; chr(0xBB) =&gt; &apos;&amp;raquo;&apos;,<br>&#xA0; &#xA0; );<br>&#xA0; &#xA0; return html_entity_decode(mb_convert_encoding(strtr($text, $map), &apos;UTF-8&apos;, &apos;ISO-8859-2&apos;), ENT_QUOTES, &apos;UTF-8&apos;);<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve been trying to find the charset of a norwegian (with a lot of &#xF8;, &#xE6;, &#xE5;) txt file written on a Mac, i&apos;ve found it in this way:
<br>
<br><span class="default">&lt;?php
<br>$text </span><span class="keyword">= </span><span class="string">&quot;A strange string to pass, maybe with some &#xF8;, &#xE6;, &#xE5; characters.&quot;</span><span class="keyword">;
<br>
<br>foreach(</span><span class="default">mb_list_encodings</span><span class="keyword">() as </span><span class="default">$chr</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">mb_convert_encoding</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="default">$chr</span><span class="keyword">).</span><span class="string">&quot; : &quot;</span><span class="keyword">.</span><span class="default">$chr</span><span class="keyword">.</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;&#xA0; &#xA0; 
<br> } 
<br></span><span class="default">?&gt;
<br></span>
<br>The line that looks good, gives you the encoding it was written in.
<br>
<br>Hope can help someone</span>
</div>
  

#


<div class="phpcode"><span class="html">
many people below talk about using 
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; mb_convert_encode</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">,</span><span class="string">&apos;HTML-ENTITIES&apos;</span><span class="keyword">,</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>to convert non-ascii code into html-readable stuff.&#xA0; Due to my webserver being out of my control, I was unable to set the database character set, and whenever PHP made a copy of my $s variable that it had pulled out of the database, it would convert it to nasty latin1 automatically and not leave it in it&apos;s beautiful UTF-8 glory.
<br>
<br>So [insert korean characters here] turned into ?????.
<br>
<br>I found myself needing to pass by reference (which of course is deprecated/nonexistent in recent versions of PHP)
<br>so instead of
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; mb_convert_encode</span><span class="keyword">(&amp;</span><span class="default">$s</span><span class="keyword">,</span><span class="string">&apos;HTML-ENTITIES&apos;</span><span class="keyword">,</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>which worked perfectly until I upgraded, so I had to use
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; call_user_func_array</span><span class="keyword">(</span><span class="string">&apos;mb_convert_encoding&apos;</span><span class="keyword">, array(&amp;</span><span class="default">$s</span><span class="keyword">,</span><span class="string">&apos;HTML-ENTITIES&apos;</span><span class="keyword">,</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>Hope it helps someone else out</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hey guys. For everybody who&apos;s looking for a function that is converting an iso-string to utf8 or an utf8-string to iso, here&apos;s your solution:<br><br>public function encodeToUtf8($string) {<br>&#xA0; &#xA0;&#xA0; return mb_convert_encoding($string, &quot;UTF-8&quot;, mb_detect_encoding($string, &quot;UTF-8, ISO-8859-1, ISO-8859-15&quot;, true));<br>}<br><br>public function encodeToIso($string) {<br>&#xA0; &#xA0;&#xA0; return mb_convert_encoding($string, &quot;ISO-8859-1&quot;, mb_detect_encoding($string, &quot;UTF-8, ISO-8859-1, ISO-8859-15&quot;, true));<br>}<br><br>For me these functions are working fine. Give it a try</span>
</div>
  

#


<div class="phpcode"><span class="html">
aaron, to discard unsupported characters instead of printing a ?, you might as well simply set the configuration directive:<br><br>mbstring.substitute_character = &quot;none&quot;<br><br>in your php.ini. Be sure to include the quotes around none. Or at run-time with<br><br><span class="default">&lt;?php<br>ini_set</span><span class="keyword">(</span><span class="string">&apos;mbstring.substitute_character&apos;</span><span class="keyword">, </span><span class="string">&quot;none&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-convert-encoding.php)

**[⬆ to root](/)**