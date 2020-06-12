# gmp_gcd




<div class="phpcode"><span class="html">
here is an elegant recursive solution<br><span class="default">&lt;?php&#xA0; &#xA0; <br><br></span><span class="keyword">function </span><span class="default">gcd</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">,</span><span class="default">$b</span><span class="keyword">) {<br>&#xA0; &#xA0; return (</span><span class="default">$a </span><span class="keyword">% </span><span class="default">$b</span><span class="keyword">) ? </span><span class="default">gcd</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">,</span><span class="default">$a </span><span class="keyword">% </span><span class="default">$b</span><span class="keyword">) : </span><span class="default">$b</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gmp-gcd.php)

**[⬆ to root](/)**