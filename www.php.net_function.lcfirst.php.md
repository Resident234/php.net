# lcfirst




<div class="phpcode"><span class="html">
Easiest work-around I&apos;ve found for &lt;5.3:<br><span class="default">&lt;?php<br><br>$string </span><span class="keyword">= </span><span class="string">&quot;CamelCase&quot;<br></span><span class="default">$string</span><span class="keyword">{</span><span class="default">0</span><span class="keyword">} = </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">{</span><span class="default">0</span><span class="keyword">})<br>echo </span><span class="default">$string</span><span class="keyword">; </span><span class="comment">// outputs camelCase<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.lcfirst.php)

**[To root](/README.md)**