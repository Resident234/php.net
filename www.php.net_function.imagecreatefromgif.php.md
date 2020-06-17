# imagecreatefromgif




<div class="phpcode"><span class="html">
An updated is_ani based on issues reported here and elsewhere<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">is_ani</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">) {<br>&#xA0; &#xA0; if(!(</span><span class="default">$fh </span><span class="keyword">= @</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">)))<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$count </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="comment">//an animated gif contains multiple &quot;frames&quot;, with each frame having a <br>&#xA0; &#xA0; //header made up of:<br>&#xA0; &#xA0; // * a static 4-byte sequence (\x00\x21\xF9\x04)<br>&#xA0; &#xA0; // * 4 variable bytes<br>&#xA0; &#xA0; // * a static 2-byte sequence (\x00\x2C) (some variants may use \x00\x21 ?)<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; // We read through the file til we reach the end of the file, or we&apos;ve found <br>&#xA0; &#xA0; // at least 2 frame headers<br>&#xA0; &#xA0; </span><span class="keyword">while(!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">) &amp;&amp; </span><span class="default">$count </span><span class="keyword">&lt; </span><span class="default">2</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$chunk </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, </span><span class="default">1024 </span><span class="keyword">* </span><span class="default">100</span><span class="keyword">); </span><span class="comment">//read 100kb at a time<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$count </span><span class="keyword">+= </span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="string">&apos;#\x00\x21\xF9\x04.{4}\x00(\x2C|\x21)#s&apos;</span><span class="keyword">, </span><span class="default">$chunk</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);<br>&#xA0;&#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$count </span><span class="keyword">&gt; </span><span class="default">1</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromgif.php)

**[To root](/README.md)**