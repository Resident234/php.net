# The RegexIterator class




<div class="phpcode"><span class="html">
An exemple :
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= new </span><span class="default">ArrayIterator</span><span class="keyword">(array(</span><span class="string">&apos;test1&apos;</span><span class="keyword">, </span><span class="string">&apos;test2&apos;</span><span class="keyword">, </span><span class="string">&apos;test3&apos;</span><span class="keyword">));
<br></span><span class="default">$i </span><span class="keyword">= new </span><span class="default">RegexIterator</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="string">&apos;/^(test)(\d+)/&apos;</span><span class="keyword">, </span><span class="default">RegexIterator</span><span class="keyword">::</span><span class="default">REPLACE</span><span class="keyword">);
<br></span><span class="default">$i</span><span class="keyword">-&gt;</span><span class="default">replacement </span><span class="keyword">= </span><span class="string">&apos;$2:$1&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0;&#xA0; 
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">iterator_to_array</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">));
<br></span><span class="comment">/*
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; 1:test
<br>&#xA0; &#xA0; [1] =&gt; 2:test
<br>&#xA0; &#xA0; [2] =&gt; 3:test
<br>)
<br>*/
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.regexiterator.php)

**[⬆ to root](/)**