# 




<div class="phpcode"><span class="html">
Looping through letters is possible. I&apos;m amazed at how few people know that.<br><br>for($col = &apos;R&apos;; $col != &apos;AD&apos;; $col++) {<br>&#xA0; &#xA0; echo $col.&apos; &apos;;<br>}<br><br>returns: R S T U V W X Y Z AA AB AC<br><br>Take note that you can&apos;t use $col &lt; &apos;AD&apos;. It only works with !=<br>Very convenient when working with excel columns.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The point about the speed in loops is, that the middle and the last expression are executed EVERY time it loops.
<br>So you should try to take everything that doesn&apos;t change out of the loop.
<br>Often you use a function to check the maximum of times it should loop. Like here:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">somewhat_calcMax</span><span class="keyword">(); </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; </span><span class="default">somewhat_doSomethingWith</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Faster would be:
<br>
<br><span class="default">&lt;?php
<br>$maxI </span><span class="keyword">= </span><span class="default">somewhat_calcMax</span><span class="keyword">();
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">$maxI</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; </span><span class="default">somewhat_doSomethingWith</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>And here a little trick:
<br>
<br><span class="default">&lt;?php
<br>$maxI </span><span class="keyword">= </span><span class="default">somewhat_calcMax</span><span class="keyword">();
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">$maxI</span><span class="keyword">; </span><span class="default">somewhat_doSomethingWith</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">++)) ;
<br></span><span class="default">?&gt;
<br></span>
<br>The $i gets changed after the copy for the function (post-increment).</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use strtotime with for loops to loop through dates<br><br><span class="default">&lt;?php<br></span><span class="keyword">for (</span><span class="default">$date </span><span class="keyword">= </span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&quot;2014-01-01&quot;</span><span class="keyword">); </span><span class="default">$date </span><span class="keyword">&lt; </span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&quot;2014-02-01&quot;</span><span class="keyword">); </span><span class="default">$date </span><span class="keyword">= </span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&quot;+1 day&quot;</span><span class="keyword">, </span><span class="default">$date</span><span class="keyword">)) {<br>&#xA0; &#xA0; echo </span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">, </span><span class="default">$date</span><span class="keyword">).</span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.for.php)

**[To root](/)**