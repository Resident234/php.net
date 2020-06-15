# DateTime::__constructdate_create




<div class="phpcode"><span class="html">
There&apos;s a reason for ignoring the time zone when you pass a timestamp to __construct.&#xA0; That is, UNIX timestamps are by definition based on UTC.&#xA0; @1234567890 represents the same date/time regardless of time zone.&#xA0; So there&apos;s no need for a time zone at all.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The theoretical limits of the date range seem to be &quot;-9999-01-01&quot; through &quot;9999-12-31&quot; (PHP 5.2.9 on Windows Vista 64):<br><br><span class="default">&lt;?php<br><br>$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;9999-12-31&quot;</span><span class="keyword">); <br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">); </span><span class="comment">// &quot;9999-12-31&quot;<br><br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;0000-12-31&quot;</span><span class="keyword">); <br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">); </span><span class="comment">// &quot;0000-12-31&quot;<br><br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;-9999-12-31&quot;</span><span class="keyword">); <br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">); </span><span class="comment">// &quot;-9999-12-31&quot;<br><br></span><span class="default">?&gt;<br></span><br>Dates above 10000 and below -10000 do not throw errors but produce weird results:<br><br><span class="default">&lt;?php<br><br>$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;10019-01-01&quot;</span><span class="keyword">); <br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">); </span><span class="comment">// &quot;2009-01-01&quot;<br><br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;10009-01-01&quot;</span><span class="keyword">); <br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">); </span><span class="comment">// &quot;2009-01-01&quot;<br><br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;-10019-01-01&quot;</span><span class="keyword">); <br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">); </span><span class="comment">// &quot;2009-01-01&quot;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A definite &quot;gotcha&quot; (while documented) that exists in the __construct is that it ignores your timezone if the $time is a timestamp.&#xA0; While this may not make sense, the object does provide you with methods to work around it.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// New Timezone Object
<br></span><span class="default">$timezone </span><span class="keyword">= new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;America/New_York&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// New DateTime Object
<br></span><span class="default">$date </span><span class="keyword">=&#xA0; new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;@1306123200&apos;</span><span class="keyword">, </span><span class="default">$timezone</span><span class="keyword">);&#xA0; &#xA0; 
<br>
<br></span><span class="comment">// You would expect the date to be 2011-05-23 00:00:00
<br>// But it actually outputs 2011-05-23 04:00:00
<br></span><span class="keyword">echo </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// You can still set the timezone though like so...&#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">setTimezone</span><span class="keyword">(</span><span class="default">$timezone</span><span class="keyword">);
<br>
<br></span><span class="comment">// This will now output 2011-05-23 00:00:00
<br></span><span class="keyword">echo </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.construct.php)

**[To root](/README.md)**