# sleep




<div class="phpcode"><span class="html">
This may seem obvious, but I thought I would save someone from something that just confused me: you cannot use sleep() to sleep for fractions of a second. This:<br><br><span class="default">&lt;?php sleep</span><span class="keyword">(</span><span class="default">0.25</span><span class="keyword">) </span><span class="default">?&gt;<br></span><br>will not work as expected. The 0.25 is cast to an integer, so this is equivalent to sleep(0). To sleep for a quarter of a second, use:<br><br><span class="default">&lt;?php usleep</span><span class="keyword">(</span><span class="default">250000</span><span class="keyword">) </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
re: &quot;mitigating the chances of a full bruit force attack by a limit of 30 lookups a minute.&quot;<br><br>Not really - the attacker could do 100 requests. Each request might take 2 seconds but it doesn&apos;t stop the number of requests done. You need to stop processing more than one request every 2 seconds rather than delay it by 2 seconds on each execution.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Maybe obvious, but this my function to delay script execution using decimals for seconds (to mimic sleep(1.5) for example):<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Delays execution of the script by the given time.<br> * @param mixed $time Time to pause script execution. Can be expressed<br> * as an integer or a decimal.<br> * @example msleep(1.5); // delay for 1.5 seconds<br> * @example msleep(.1); // delay for 100 milliseconds<br> */<br></span><span class="keyword">function </span><span class="default">msleep</span><span class="keyword">(</span><span class="default">$time</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">usleep</span><span class="keyword">(</span><span class="default">$time </span><span class="keyword">* </span><span class="default">1000000</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You should put sleep into both the pass and fail branches, since an attacker can check whether the response is slow and use that as an indicator - cutting down the delay time. But a delay in both branches eliminates this possibility.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note: The set_time_limit() function and the configuration directive max_execution_time only affect the execution time of the script itself. Any time spent on activity that happens outside the execution of the script such as system calls using system(), the sleep() function, database queries, etc. is not included when determining the maximum time that the script has been running.</span>
</div>
  

#


<div class="phpcode"><span class="html">
it is a bad idea to use sleep() for delayed output effects as
<br>
<br>1) you have to flush() output before you sleep
<br>
<br>2) depending on your setup flush() will not work all the way to the browser as the web server might apply buffering of its own or the browser might not render output it thinks not to be complete
<br>
<br>netscape for example will only display complete lines and will not show table parts until the &lt;/table&gt; tag arrived
<br>
<br>so use sleep if you have to wait&#xA0; for events and don&apos;t want to burn&#xA0; to much cycles, but don&apos;t use it for silly delayed output effects!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.sleep.php)

**[To root](/README.md)**