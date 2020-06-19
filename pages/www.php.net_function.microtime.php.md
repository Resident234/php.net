# microtime




<div class="phpcode"><span class="html">
You can use one variable to check execution $time as follow:
<br>
<br><span class="default">&lt;?php
<br>$time </span><span class="keyword">= -</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">$hash </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">rand</span><span class="keyword">(</span><span class="default">1000</span><span class="keyword">,</span><span class="default">4000</span><span class="keyword">); ++</span><span class="default">$i</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$hash </span><span class="keyword">^= </span><span class="default">md5</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">str_shuffle</span><span class="keyword">(</span><span class="string">&quot;0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ&quot;</span><span class="keyword">), </span><span class="default">0</span><span class="keyword">, </span><span class="default">rand</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">10</span><span class="keyword">)));
<br>}
<br></span><span class="default">$time </span><span class="keyword">+= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>echo </span><span class="string">&quot;Hash: </span><span class="default">$hash</span><span class="string"> iterations:</span><span class="default">$i</span><span class="string"> time: &quot;</span><span class="keyword">,</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;%f&apos;</span><span class="keyword">, </span><span class="default">$time</span><span class="keyword">),</span><span class="default">PHP_EOL</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that the timestamp returned is &quot;with microseconds&quot;, not &quot;in microseconds&quot;. This is especially good to know if you pass &apos;true&apos; as the parameter and then calculate the difference between two float values -- the result is already in seconds; it doesn&apos;t need to be divided by a million.</span>
</div>
  

#


<div class="phpcode"><span class="html">
All these timing scripts rely on microtime which relies on gettimebyday(2)<br><br>This can be inaccurate on servers that run ntp to syncronise the servers<br>time.<br><br>For timing, you should really use clock_gettime(2) with the<br>CLOCK_MONOTONIC flag set.<br><br>This returns REAL WORLD time, not affected by intentional clock drift.<br><br>This may seem a bit picky, but I recently saw a server that&apos;s clock was an<br>hour out, and they&apos;d set it to &apos;drift&apos; to the correct time (clock is speeded<br>up until it reaches the correct time)<br><br>Those sorts of things can make a real impact.<br><br>Any solutions, seeing as php doesn&apos;t have a hook into clock_gettime?<br><br>More info here: <a href="http://tinyurl.com/28vxja9" rel="nofollow" target="_blank">http://tinyurl.com/28vxja9</a><br><br><a href="http://blog.habets.pp.se/2010/09/" rel="nofollow" target="_blank">http://blog.habets.pp.se/2010/09/</a><br>gettimeofday-should-never-be-used-to-measure-time</span>
</div>
  

#


<div class="phpcode"><span class="html">
A lot of the comments here suggest adding in the following way:&#xA0; (float)$usec + (float)$sec<br>Make sure you have the float precision high enough as with the default precision of 12, you are only precise to the 0.01 seconds.&#xA0; <br>Set this in you php.ini file.<br>&#xA0; &#xA0; &#xA0; &#xA0; precision&#xA0; &#xA0; =&#xA0; 16</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.microtime.php)

**[To root](/README.md)**