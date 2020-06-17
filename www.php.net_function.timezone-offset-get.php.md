# timezone_offset_get




<div class="phpcode"><span class="html">
Hi,<br>This might help someone with timezone differences. I wanted to find the offset in seconds between my timezone and a remote timezone so I wrote this function and it seems to work well for me.<br><br><span class="default">&lt;?php<br></span><span class="comment">/**&#xA0; &#xA0; Returns the offset from the origin timezone to the remote timezone, in seconds.<br>*&#xA0; &#xA0; @param $remote_tz;<br>*&#xA0; &#xA0; @param $origin_tz; If null the servers current timezone is used as the origin.<br>*&#xA0; &#xA0; @return int;<br>*/<br></span><span class="keyword">function </span><span class="default">get_timezone_offset</span><span class="keyword">(</span><span class="default">$remote_tz</span><span class="keyword">, </span><span class="default">$origin_tz </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; if(</span><span class="default">$origin_tz </span><span class="keyword">=== </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$origin_tz </span><span class="keyword">= </span><span class="default">date_default_timezone_get</span><span class="keyword">())) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">; </span><span class="comment">// A UTC timestamp was returned -- bail out!<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">$origin_dtz </span><span class="keyword">= new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="default">$origin_tz</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$remote_dtz </span><span class="keyword">= new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="default">$remote_tz</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$origin_dt </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;now&quot;</span><span class="keyword">, </span><span class="default">$origin_dtz</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$remote_dt </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;now&quot;</span><span class="keyword">, </span><span class="default">$remote_dtz</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$offset </span><span class="keyword">= </span><span class="default">$origin_dtz</span><span class="keyword">-&gt;</span><span class="default">getOffset</span><span class="keyword">(</span><span class="default">$origin_dt</span><span class="keyword">) - </span><span class="default">$remote_dtz</span><span class="keyword">-&gt;</span><span class="default">getOffset</span><span class="keyword">(</span><span class="default">$remote_dt</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$offset</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span>Examples:<br><span class="default">&lt;?php<br></span><span class="comment">// This will return 10800 (3 hours) ...<br></span><span class="default">$offset </span><span class="keyword">= </span><span class="default">get_timezone_offset</span><span class="keyword">(</span><span class="string">&apos;America/Los_Angeles&apos;</span><span class="keyword">,</span><span class="string">&apos;America/New_York&apos;</span><span class="keyword">);<br></span><span class="comment">// or, if your server time is already set to &apos;America/New_York&apos;...<br></span><span class="default">$offset </span><span class="keyword">= </span><span class="default">get_timezone_offset</span><span class="keyword">(</span><span class="string">&apos;America/Los_Angeles&apos;</span><span class="keyword">);<br></span><span class="comment">// You can then take $offset and adjust your timestamp.<br></span><span class="default">$offset_time </span><span class="keyword">= </span><span class="default">time</span><span class="keyword">() + </span><span class="default">$offset</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Happy Coding!!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.timezone-offset-get.php)

**[To root](/README.md)**