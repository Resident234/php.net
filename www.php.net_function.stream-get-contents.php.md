# stream_get_contents




<div class="phpcode"><span class="html">
It is important to know that stream_get_contents behaves differently with different versions of PHP. Consider the following<br><br><span class="default">&lt;?php<br><br>$handle </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;file&apos;</span><span class="keyword">, </span><span class="string">&apos;w+&apos;</span><span class="keyword">); </span><span class="comment">// truncate + attempt to create<br></span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="string">&apos;12345&apos;</span><span class="keyword">); </span><span class="comment">// file position &gt; 0<br></span><span class="default">rewind</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">); </span><span class="comment">// position = 0<br></span><span class="default">$content </span><span class="keyword">= </span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">); </span><span class="comment">// file position = 0 in PHP 5.1.6, file position &gt; 0 in PHP 5.2.17!<br></span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="string">&apos;6789&apos;</span><span class="keyword">);<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);<br><br></span><span class="comment">/**<br> *<br> * &apos;file&apos; content<br> * <br> * PHP 5.1.6:<br> * 67895<br> *<br> * PHP 5.2.17:<br> * 123456789<br> *<br> */<br></span><span class="default">?&gt;<br></span><br>As a result, stream_get_contents() affects file position in 5.1, and do not affect file position in 5.2 or better.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-get-contents.php)

**[To root](/README.md)**