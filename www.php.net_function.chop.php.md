# chop




<div class="phpcode"><span class="html">
Rather use rtrim(). Usage of chop() is not very clear nor consistent for people reading the code after you.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are searching for a function that does the same trick as chop in PERL, then you should just do the following code:<br><span class="default">&lt;?php<br>&#xA0;&#xA0; $str </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The question is: why isn&apos;t chop() an alias of the code above, rather than something which will trap developpers?</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.chop.php)

**[To root](/README.md)**