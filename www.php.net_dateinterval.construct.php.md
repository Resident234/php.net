# DateInterval::__construct




<div class="phpcode"><span class="html">
M is used to indicate both months and minutes.<br><br>As noted on the referenced wikipedia page for ISO 6801 <a href="http://en.wikipedia.org/wiki/Iso8601#Durations" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Iso8601#Durations</a><br><br>To resolve ambiguity, &quot;P1M&quot; is a one-month duration and &quot;PT1M&quot; is a one-minute duration (note the time designator, T, that precedes the time value).<br><br>Using: PHP 5.3.2-1ubuntu4.19<br><br>// For 3 Months<br>$dateTime = new DateTime;echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>$dateTime-&gt;add(new DateInterval(&quot;P3M&quot;));<br>echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>Results in:<br>2013-07-11T11:12:26-0400<br>2013-10-11T11:12:26-0400<br><br>// For 3 Minutes<br>$dateTime = new DateTime;echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>$dateTime-&gt;add(new DateInterval(&quot;PT3M&quot;));<br>echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>Results in:<br>2013-07-11T11:12:42-0400<br>2013-07-11T11:15:42-0400<br><br>Insert a T after the P in the interval to add 3 minutes instead of 3 months.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I think it is easiest if you would just use the sub method on the DateTime class.<br><br><span class="default">&lt;?php<br>$date </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">();<br></span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">sub</span><span class="keyword">(new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&quot;P89D&quot;</span><span class="keyword">));</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that, while a DateInterval object has an $invert property, you cannot supply a negative directly to the constructor similar to specifying a negative in XSD (&quot;-P1Y&quot;). You will get an exception through if you do this. <br><br>Instead you need to construct using a positive interval (&quot;P1Y&quot;) and the specify the $invert property === 1.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be noted that this class will not calculate days/hours/minutes/seconds etc given a value in a single denomination of time.&#xA0; For example:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $di </span><span class="keyword">= new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&apos;PT3600S&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">$di</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;%H:%i:%s&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; <br></span><span class="default">?&gt;<br></span><br>will yield 0:0:3600 instead of the expected 1:0:0</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/dateinterval.construct.php)

**[To root](/README.md)**