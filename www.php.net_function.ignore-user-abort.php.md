# ignore_user_abort




<div class="phpcode"><span class="html">
Comment from Anonymous is not 100% valid. Time from sleep function is not counted to execution time because sleep delays program execution (see <a href="http://www.php.net/manual/en/function.sleep.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.sleep.php</a> and comments). We tested it and it&apos;s true. Try this:<br><br><span class="default">&lt;?php<br><br>set_time_limit</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">);<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">);<br>echo </span><span class="string">&apos;hi!&apos;</span><span class="keyword">;<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">);<br>echo </span><span class="string">&apos;bye bye!&apos;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>It means, that if the loop most of the time will be at sleep (and in this case it probably be), then this script may be active for months or years even if you set time limit to one day.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to simulate a crontask you must call this script once and it will keep running forever (during server uptime) in the background while &quot;doing something&quot; every specified seconds (= $interval):
<br>
<br><span class="default">&lt;?php
<br>ignore_user_abort</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">); </span><span class="comment">// run script in background
<br></span><span class="default">set_time_limit</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">); </span><span class="comment">// run script forever
<br></span><span class="default">$interval</span><span class="keyword">=</span><span class="default">60</span><span class="keyword">*</span><span class="default">15</span><span class="keyword">; </span><span class="comment">// do every 15 minutes...
<br></span><span class="keyword">do{
<br>&#xA0;&#xA0; </span><span class="comment">// add the script that has to be ran every 15 minutes here
<br>&#xA0;&#xA0; // ...
<br>&#xA0;&#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">); </span><span class="comment">// wait 15 minutes
<br></span><span class="keyword">}while(</span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
use the spiritual-coder&apos;s code below with exreme caution and do not use it unless you are an advanced user.<br><br>first of all, such a code with no bypass point can cause infinite loops and ghost threads in server. there must be a trick to break out the loop. <br><br>i.e. you can use&#xA0; if (file_exists(dirname(__FILE__).&quot;stop.txt&quot;)) break; in the loop so if you create &quot;stop.txt&quot;, she script will stop execution.<br><br>also if you use set_time_limit(86400); instead of set_time_limit(0); your script will stop after one day.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ignore-user-abort.php)

**[To root](/README.md)**