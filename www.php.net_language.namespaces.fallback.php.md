# Using namespaces: fallback to global function/constant




<div class="phpcode"><span class="html">
You can use the fallback policy to provide mocks for built-in functions like time(). You therefore have to call those functions unqualified:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">;<br><br>function </span><span class="default">time</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">1234</span><span class="keyword">;<br>}<br><br></span><span class="default">assert </span><span class="keyword">(</span><span class="default">1234 </span><span class="keyword">== </span><span class="default">time</span><span class="keyword">());<br></span><span class="default">?&gt;<br></span><br>However there&apos;s a restriction that you have to define the mock function before the first usage in the tested class method. This is documented in Bug #68541.<br><br>You can find the mock library php-mock at GitHub.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.fallback.php)

**[To root](/)**