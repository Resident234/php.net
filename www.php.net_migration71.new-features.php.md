# New features




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">swap</span><span class="keyword">( &amp;</span><span class="default">$a</span><span class="keyword">, &amp;</span><span class="default">$b </span><span class="keyword">): </span><span class="default">void<br>&#xA0; </span><span class="keyword">{ [ </span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">] = [ </span><span class="default">$b</span><span class="keyword">, </span><span class="default">$a </span><span class="keyword">]; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that declaring nullable return type does not mean that you can skip return statement at all. For example:<br><br>php &gt; function a(): ?string { }<br>php &gt; a();<br>PHP Warning:&#xA0; Uncaught TypeError: Return value of a() must be of the type string or null, none returned in php shell code:2<br><br>php &gt; function b(): ?string { return; }<br>PHP Fatal error:&#xA0; A function with return type must return a value (did you mean &quot;return null;&quot; instead of &quot;return;&quot;?) in php shell code on line 2</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration71.new-features.php)

**[⬆ to root](/)**