# strtoupper




<div class="phpcode"><span class="html">
One might think that setting the correct locale would do the trick with for example german umlauts, but this is not the case. You have to use mb_strtoupper() instead:<br><br><span class="default">&lt;?php<br><br>setlocale</span><span class="keyword">(</span><span class="default">LC_CTYPE</span><span class="keyword">, </span><span class="string">&apos;de_DE.UTF8&apos;</span><span class="keyword">);<br><br>echo </span><span class="default">strtoupper</span><span class="keyword">(</span><span class="string">&apos;Umlaute &#xE4;&#xF6;&#xFC; in uppercase&apos;</span><span class="keyword">); </span><span class="comment">// outputs &quot;UMLAUTE &#xE4;&#xF6;&#xFC; IN UPPERCASE&quot;<br></span><span class="keyword">echo </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="string">&apos;Umlaute &#xE4;&#xF6;&#xFC; in uppercase&apos;</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">); </span><span class="comment">// outputs &quot;UMLAUTE &#xC4;&#xD6;&#xDC; IN UPPERCASE&quot;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strtoupper.php)

**[To root](/README.md)**