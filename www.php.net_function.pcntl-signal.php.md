# pcntl_signal




<div class="phpcode"><span class="html">
If you are using PHP &gt;= 7.1 then DO NOT use `declare(ticks=1)` instead use `pcntl_async_signals(true)`<br><br>There&apos;s no performance hit or overhead with `pcntl_async_signals()`. See this blog post <a href="https://blog.pascal-martin.fr/post/php71-en-other-new-things.html" rel="nofollow" target="_blank">https://blog.pascal-martin.fr/post/php71-en-other-new-things.html</a> for a simple example of how to use this.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Remember that signal handlers are called immediately, so every time when your process receives registered signal blocking functions like sleep() and usleep() are interrupted (or more like ended) like it was their time to finish.<br><br>This is expected behaviour, but not mentioned in this documentation. Sleep interruption can be very unfortunate when for example you run a daemon that is supposed to execute some important task exactly every X seconds/minutes - this task could be called too early or too often and it would be hard to debug why it&apos;s happening exactly.<br><br>Simply remember that signal handlers might interfere with other parts of your code.<br>Here is example workaround ensuring that function gets executed every minute no matter how many times our loop gets interrupted by incoming signal (SIGUSR1 in this case).<br><br><span class="default">&lt;?php<br><br>&#xA0; pcntl_async_signals</span><span class="keyword">(</span><span class="default">TRUE</span><span class="keyword">);<br><br>&#xA0; </span><span class="default">pcntl_signal</span><span class="keyword">(</span><span class="default">SIGUSR1</span><span class="keyword">, function(</span><span class="default">$signal</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// do something when signal is called<br>&#xA0; </span><span class="keyword">});<br><br>&#xA0; function </span><span class="default">everyMinute</span><span class="keyword">() {<br>&#xA0; &#xA0; </span><span class="comment">// do some important stuff<br>&#xA0; </span><span class="keyword">}<br><br>&#xA0; </span><span class="default">$wait </span><span class="keyword">= </span><span class="default">60</span><span class="keyword">;<br>&#xA0; </span><span class="default">$next </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br><br>&#xA0; while (</span><span class="default">TRUE</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$stamp </span><span class="keyword">= </span><span class="default">time</span><span class="keyword">();<br>&#xA0; &#xA0; do {<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">$stamp </span><span class="keyword">&gt;= </span><span class="default">$next</span><span class="keyword">) { break; }<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$diff </span><span class="keyword">= </span><span class="default">$next </span><span class="keyword">- </span><span class="default">$stamp</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">$diff</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$stamp </span><span class="keyword">= </span><span class="default">time</span><span class="keyword">();<br>&#xA0; &#xA0; } while (</span><span class="default">$stamp </span><span class="keyword">&lt; </span><span class="default">$next</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">everyMinute</span><span class="keyword">();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$next </span><span class="keyword">= </span><span class="default">$stamp </span><span class="keyword">+ </span><span class="default">$wait</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">$wait</span><span class="keyword">);<br>&#xA0; }<br><br></span><span class="default">?&gt;<br></span><br>So in this infinite loop do {} while() calculates and adds missing sleep() to ensure that our everyMinute() function is not called too early. Both sleep() functions here are covered so everyMinute() will never be executed before it&apos;s time even if process receives multiple SIGUSR1 signals during it&apos;s runtime.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For PHP &gt;= 5.3.0, instead of declare(ticks = 1), you should now use pcntl_ signal_ dispatch().</span>
</div>
  

#


<div class="phpcode"><span class="html">
Remember that declaring a tick handler can become really expensive in terms of CPU cycles: Every n ticks the signal handling overhead will be executed. <br><br>So instead of declaring tick=1, test if tick=100 can do the job. If so, you are likely to gain fivefold speed.<br><br>As your script might always might miss some signals due to blocking operations like cURL downloads, call pcntl_signal_dispatch() on vital spots, e.g. at the beginning of your main loop.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You cannot assign a signal handler for SIGKILL (kill -9).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.pcntl-signal.php)

**[⬆ to root](/)**