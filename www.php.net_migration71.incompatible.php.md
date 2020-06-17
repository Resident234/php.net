# Backward incompatible changes




<div class="phpcode"><span class="html">
For anyone migrating from 5.x to 7.1:<br><br>About &quot;Array ordering when elements are automatically created during by reference assignments has changed&quot; on this page<br><br>(<a href="http://php.net/manual/en/migration71.incompatible.php#migration71.incompatible.array-order" rel="nofollow" target="_blank">http://php.net/manual/en/migration71.incompatible.php#migration71.incompatible.array-order</a>)<br><br>The behaviour of 7.1 is THE SAME as of PHP 5. It is only 7.0 that differs.<br><br>See <a href="https://3v4l.org/frbUc" rel="nofollow" target="_blank">https://3v4l.org/frbUc</a><br><br><span class="default">&lt;?php<br><br>$array </span><span class="keyword">= [];<br></span><span class="default">$array</span><span class="keyword">[</span><span class="string">&quot;a&quot;</span><span class="keyword">] =&amp; </span><span class="default">$array</span><span class="keyword">[</span><span class="string">&quot;b&quot;</span><span class="keyword">];<br></span><span class="default">$array</span><span class="keyword">[</span><span class="string">&quot;b&quot;</span><span class="keyword">] = </span><span class="default">1</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The backwards incompatible change &apos;The empty index operator is not supported for strings anymore&apos; has a lot more implications than just a fatal error on the following code<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">$a</span><span class="keyword">[] = </span><span class="string">&quot;hello world&quot;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This will give a fatal error in 7.1 but will work as expected in 7.0 or below and give you: (no notice, no warning)<br><br>array(1) {<br>&#xA0; [0]=&gt;<br>&#xA0; string(11) &quot;hello world&quot;<br>}<br><br>However, the following is also changed:<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">$a</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="string">&quot;hello world&quot;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br></span><span class="comment">// 7.1: string(1) &quot;h&quot;<br>// pre-7.1: array(1) {&#xA0; [0]=&gt;&#xA0; string(11) &quot;hello world&quot; }<br><br></span><span class="default">$a </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">$a</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">] = </span><span class="string">&quot;hello world&quot;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br></span><span class="comment">// 7.1: string(6) &quot;&#xA0; &#xA0;&#xA0; h&quot;<br>// pre-7.1: array(1) {&#xA0; [0]=&gt;&#xA0; string(11) &quot;hello world&quot; }<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;OMFG! Why was session.hash_function removed?!? Dude!&quot;<br><br><a href="https://wiki.php.net/rfc/session-id-without-hashing" rel="nofollow" target="_blank">https://wiki.php.net/rfc/session-id-without-hashing</a><br><br>There. Saved ya a search.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration71.incompatible.php)

**[To root](/README.md)**