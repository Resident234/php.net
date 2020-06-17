# gmdate




<div class="phpcode"><span class="html">
If you have the same application running in different countries, you may have some troubles getting the local time..<br>In my case, I was having troubles with a clock created with Macromedia Flash... the time shown by the clock was supposed to be set up by the server, passing the timestamp. When I moved the file to another country, I got a wrong time...<br>You can use the timezone offset ( date(&quot;Z&quot;) ) to handle this kind of thing...<br><br><span class="default">&lt;?php<br>$timestamp </span><span class="keyword">= </span><span class="default">time</span><span class="keyword">()+</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;Z&quot;</span><span class="keyword">);<br>echo </span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&quot;Y/m/d H:i:s&quot;</span><span class="keyword">,</span><span class="default">$timestamp</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Wath out for summer time and winter time...
<br>
<br>If you want to get the current date and time based on GMT you could use this :
<br>
<br><span class="default">&lt;?php
<br>$timezone&#xA0; </span><span class="keyword">= -</span><span class="default">5</span><span class="keyword">; </span><span class="comment">//(GMT -5:00) EST (U.S. &amp; Canada)
<br></span><span class="keyword">echo </span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&quot;Y/m/j H:i:s&quot;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">() + </span><span class="default">3600</span><span class="keyword">*(</span><span class="default">$timezone</span><span class="keyword">+</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;I&quot;</span><span class="keyword">))); 
<br></span><span class="default">?&gt;
<br></span>
<br>this would gives: 2004/07/8 14:35:19 in summer time
<br>and 2004/07/8 13:35:19 in winter time.
<br>
<br>Note that date(&quot;I&quot;) returns 1 in summer and 0 in winter.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For an RFC 1123 (HTTP header date) date, try:
<br>
<br><span class="default">&lt;?php
<br>$rfc_1123_date </span><span class="keyword">= </span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&apos;D, d M Y H:i:s T&apos;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">());
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For me most of the examples here needed the + or - seconds to set the time zone. I wanted a faster way to get the time zone in seconds. So I created this : <br><span class="default">&lt;?php <br>$h </span><span class="keyword">= </span><span class="string">&quot;3&quot;</span><span class="keyword">;</span><span class="comment">// Hour for time zone goes here e.g. +7 or -4, just remove the + or -<br></span><span class="default">$hm </span><span class="keyword">= </span><span class="default">$h </span><span class="keyword">* </span><span class="default">60</span><span class="keyword">; <br></span><span class="default">$ms </span><span class="keyword">= </span><span class="default">$hm </span><span class="keyword">* </span><span class="default">60</span><span class="keyword">;<br></span><span class="default">$gmdate </span><span class="keyword">= </span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&quot;m/d/Y g:i:s A&quot;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">()-(</span><span class="default">$ms</span><span class="keyword">)); </span><span class="comment">// the &quot;-&quot; can be switched to a plus if that&apos;s what your time zone is.<br></span><span class="keyword">echo </span><span class="string">&quot;Your current time now is :&#xA0; </span><span class="default">$gmdate</span><span class="string"> . &quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span> <br>It works. Hope it helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gmdate.php)

**[To root](/README.md)**