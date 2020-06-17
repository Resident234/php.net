# The DateTime class




<div class="phpcode"><span class="html">
DateTime supports microseconds since 5.2.2. This is mentioned in the documentation for the date function, but bears repeating here. You can create a DateTime with fractional seconds and retrieve that value using the &apos;u&apos; format string.<br><br><span class="default">&lt;?php<br></span><span class="comment">// Instantiate a DateTime with microseconds.<br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;2011-01-01T15:03:01.012345Z&apos;</span><span class="keyword">);<br><br></span><span class="comment">// Output the microseconds.<br></span><span class="keyword">echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;u&apos;</span><span class="keyword">); </span><span class="comment">// 012345<br><br>// Output the date with microseconds.<br></span><span class="keyword">echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d\TH:i:s.u&apos;</span><span class="keyword">); </span><span class="comment">// 2011-01-01T15:03:01.012345</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Small but powerful extension to DateTime<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Blar_DateTime </span><span class="keyword">extends </span><span class="default">DateTime </span><span class="keyword">{<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Return Date in ISO8601 format<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @return String<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__toString</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Return difference between $this and $now<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param Datetime|String $now<br>&#xA0; &#xA0;&#xA0; * @return DateInterval<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">diff</span><span class="keyword">(</span><span class="default">$now </span><span class="keyword">= </span><span class="string">&apos;NOW&apos;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!(</span><span class="default">$now </span><span class="keyword">instanceOf </span><span class="default">DateTime</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$now </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$now</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$now</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Return Age in Years<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param Datetime|String $now<br>&#xA0; &#xA0;&#xA0; * @return Integer<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">getAge</span><span class="keyword">(</span><span class="default">$now </span><span class="keyword">= </span><span class="string">&apos;NOW&apos;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$now</span><span class="keyword">)-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;%y&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">?&gt;<br></span><br>Usage:<br><br><span class="default">&lt;?php<br><br>$birthday </span><span class="keyword">= new </span><span class="default">Blar_DateTime</span><span class="keyword">(</span><span class="string">&apos;1879-03-14&apos;</span><span class="keyword">);<br><br></span><span class="comment">// Example 1<br></span><span class="keyword">echo </span><span class="default">$birthday</span><span class="keyword">;<br></span><span class="comment">// Result: 1879-03-14 00:00<br><br>// Example 2<br></span><span class="keyword">echo </span><span class="string">&apos;&lt;p&gt;Albert Einstein would now be &apos;</span><span class="keyword">, </span><span class="default">$birthday</span><span class="keyword">-&gt;</span><span class="default">getAge</span><span class="keyword">(), </span><span class="string">&apos; years old.&lt;/p&gt;&apos;</span><span class="keyword">;<br></span><span class="comment">// Result: &lt;p&gt;Albert Einstein would now be 130 years old.&lt;/p&gt;<br><br>// Example 3<br></span><span class="keyword">echo </span><span class="string">&apos;&lt;p&gt;Albert Einstein would now be &apos;</span><span class="keyword">, </span><span class="default">$birthday</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">()-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;%y Years, %m Months, %d Days&apos;</span><span class="keyword">), </span><span class="string">&apos; old.&lt;/p&gt;&apos;</span><span class="keyword">;<br></span><span class="comment">// Result: &lt;p&gt;Albert Einstein would now be 130 Years, 10 Months, 10 Days old.&lt;/p&gt;<br><br>// Example 4<br></span><span class="keyword">echo </span><span class="string">&apos;&lt;p&gt;Albert Einstein was on 2010-10-10 &apos;</span><span class="keyword">, </span><span class="default">$birthday</span><span class="keyword">-&gt;</span><span class="default">getAge</span><span class="keyword">(</span><span class="string">&apos;2010-10-10&apos;</span><span class="keyword">), </span><span class="string">&apos; years old.&lt;/p&gt;&apos;</span><span class="keyword">;<br></span><span class="comment">// Result: &lt;p&gt;Albert Einstein was on 2010-10-10 131 years old.&lt;/p&gt;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is a subtle difference between the following two statments which causes JavaScript&apos;s Date object on iPhones to fail.<br><br><span class="default">&lt;?php<br>$objDateTime </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;NOW&apos;</span><span class="keyword">);<br>echo </span><span class="default">$objDateTime</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">); </span><span class="comment">// ISO8601 formated datetime<br></span><span class="keyword">echo </span><span class="default">$objDateTime</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">ISO8601</span><span class="keyword">); </span><span class="comment">// Another way to get an ISO8601 formatted string<br><br>/**<br>On my local machine this results in: <br><br>2013-03-01T16:15:09+01:00<br>2013-03-01T16:15:09+0100<br><br>Both of these strings are valid ISO8601 datetime strings, but the latter is not accepted by the constructor of JavaScript&apos;s date object on iPhone. (Possibly other browsers as well)<br>*/<br><br></span><span class="default">?&gt;<br></span><br>Our solution was to create the following constant on our DateHelper object.<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">DateHelper<br></span><span class="keyword">{<br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * An ISO8601 format string for PHP&apos;s date functions that&apos;s compatible with JavaScript&apos;s Date&apos;s constructor method<br>&#xA0; &#xA0;&#xA0; * Example: 2013-04-12T16:40:00-04:00<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * PHP&apos;s ISO8601 constant doesn&apos;t add the colon to the timezone offset which is required for iPhone<br>&#xA0; &#xA0; **/<br>&#xA0; &#xA0; </span><span class="keyword">const </span><span class="default">ISO8601 </span><span class="keyword">= </span><span class="string">&apos;Y-m-d\TH:i:sP&apos;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
At PHP 7.1 the DateTime constructor incorporates microseconds when constructed from the current time.&#xA0; Make your comparisons carefully, since two DateTime objects constructed one after another are now more likely to have different values.<br><br><a href="http://php.net/manual/en/migration71.incompatible.php" rel="nofollow" target="_blank">http://php.net/manual/en/migration71.incompatible.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
This caused some confusion with a blog I was working on and just wanted to make other people aware of this. If you use createFromFormat to turn a date into a timestamp it will include the current time. For example:<br><br><span class="default">&lt;?php<br>$publishDate </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;m/d/Y&apos;</span><span class="keyword">, </span><span class="string">&apos;1/10/2014&apos;</span><span class="keyword">);<br>echo </span><span class="default">$publishDate</span><span class="keyword">-&gt;</span><span class="default">getTimestamp</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>Would not output the expected &quot;1389312000&quot; instead it would output something more like &quot;1389344025&quot;. To fix this you would want to do:<br><br><span class="default">&lt;?php<br>$publishDate </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;m/d/Y&apos;</span><span class="keyword">, </span><span class="string">&apos;1/10/2014&apos;</span><span class="keyword">);<br></span><span class="default">$publishDate</span><span class="keyword">-&gt;</span><span class="default">setTime</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br>echo </span><span class="default">$publishDate</span><span class="keyword">-&gt;</span><span class="default">getTimestamp</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>I hope this helps someone!</span>
</div>
  

#


<div class="phpcode"><span class="html">
This might be unexpected behavior:<br><br><span class="default">&lt;?php<br><br>$date1 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">, </span><span class="string">&apos;2017-08-31 00:00:00&apos;</span><span class="keyword">);<br></span><span class="default">$date1</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">(</span><span class="string">&apos;+1 month&apos;</span><span class="keyword">);<br><br></span><span class="comment">#or use the interval<br>#$date1-&gt;add(new DateInterval(&quot;P1M&quot;));<br><br>#will produce 2017-10-1<br>#not 2017-09-30</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
IF You want to create clone of $time, use clone..<br><br><span class="default">&lt;?php<br>&#xA0; $now&#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">;<br>&#xA0; </span><span class="default">$clone </span><span class="keyword">= </span><span class="default">$now</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//this doesnot clone so:<br>&#xA0; </span><span class="default">$clone</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">( </span><span class="string">&apos;-1 day&apos; </span><span class="keyword">);<br> <br>&#xA0; echo </span><span class="default">$now</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;d-m-Y&apos; </span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$clone</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;d-m-Y&apos; </span><span class="keyword">);<br>&#xA0; echo </span><span class="string">&apos;----&apos;</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br>&#xA0; </span><span class="comment">// will print same.. if you want to clone make like this:<br>&#xA0; </span><span class="default">$now&#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">;<br>&#xA0; </span><span class="default">$clone </span><span class="keyword">= clone </span><span class="default">$now</span><span class="keyword">;&#xA0; &#xA0; <br>&#xA0; </span><span class="default">$clone</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">( </span><span class="string">&apos;-1 day&apos; </span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; echo </span><span class="default">$now</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;d-m-Y&apos; </span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$clone</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;d-m-Y&apos; </span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Results:<br>18-07-2011<br>18-07-2011<br>----<br>19-07-2011<br>18-07-2011</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.datetime.php)

**[To root](/README.md)**