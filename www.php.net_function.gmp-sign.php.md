# gmp_sign




<div class="phpcode"><span class="html">
Hi !
<br>
<br>If you don&apos;t have the GMP extension, the sign function is really simple to code.
<br>Here an example of implementation :
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">sign</span><span class="keyword">( </span><span class="default">$number </span><span class="keyword">) {
<br>&#xA0; &#xA0; return ( </span><span class="default">$number </span><span class="keyword">&gt; </span><span class="default">0 </span><span class="keyword">) ? </span><span class="default">1 </span><span class="keyword">: ( ( </span><span class="default">$number </span><span class="keyword">&lt; </span><span class="default">0 </span><span class="keyword">) ? -</span><span class="default">1 </span><span class="keyword">: </span><span class="default">0 </span><span class="keyword">);
<br>}
<br>
<br>echo </span><span class="default">sign</span><span class="keyword">( </span><span class="default">500 </span><span class="keyword">); </span><span class="comment">// Return 1
<br></span><span class="keyword">echo </span><span class="default">sign</span><span class="keyword">( -</span><span class="default">500 </span><span class="keyword">); </span><span class="comment">// Return -1
<br></span><span class="keyword">echo </span><span class="default">sign</span><span class="keyword">( </span><span class="default">0 </span><span class="keyword">); </span><span class="comment">// Return 0
<br></span><span class="default">?&gt;
<br></span>
<br>Thomas.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gmp-sign.php)

**[⬆ to root](/)**