# DateTime::createFromFormatdate_create_from_format




<div class="phpcode"><span class="html">
Be warned that DateTime object created without explicitely providing the time portion will have the current time set instead of 00:00:00.<br><br><span class="default">&lt;?php<br>$date </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">)); </span><span class="comment">//will print 2012-10-17 13:57:34 (the current time)<br></span><span class="default">?&gt;<br></span><br>That&apos;s also why you can&apos;t safely compare equality of such DateTime objects:<br><br><span class="default">&lt;?php<br>$date1 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">);<br></span><span class="default">$date2 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date1 </span><span class="keyword">== </span><span class="default">$date2</span><span class="keyword">); </span><span class="comment">//will be false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date1 </span><span class="keyword">&gt;= </span><span class="default">$date2</span><span class="keyword">); </span><span class="comment">//will be false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date1 </span><span class="keyword">&lt; </span><span class="default">$date2</span><span class="keyword">); </span><span class="comment">//will be true<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to safely compare equality of a DateTime object without explicitly providing the time portion make use of the ! format character.<br><br><span class="default">&lt;?php<br>$date1 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;!Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">);<br></span><span class="default">$date2 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;!Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="comment">/* <br> $date1 and $date2 will both be set to a timestamp of &quot;2012-10-17 00:00:00&quot;<br> var_dump($date1 == $date2); //will be true<br> var_dump($date1 &gt; $date2); //will be false<br> var_dump($date1 &lt; $date2); //will be false<br>*/<br></span><span class="default">?&gt;<br></span><br>If you omit the ! format character without explicitly providing the time portion your timestamp which will include the current system time in the stamp.<br><br><span class="default">&lt;?php<br>$date1 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">);<br></span><span class="default">$date2 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2012-10-17&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date1 </span><span class="keyword">== </span><span class="default">$date2</span><span class="keyword">); </span><span class="comment">//will be false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date1 </span><span class="keyword">&gt;= </span><span class="default">$date2</span><span class="keyword">); </span><span class="comment">//will be false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$date1 </span><span class="keyword">&lt; </span><span class="default">$date2</span><span class="keyword">); </span><span class="comment">//will be true<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Parsing RFC3339 strings can be very tricky when their are microseconds in the date string.<br><br>Since PHP 7 there is the undocumented constant DateTime::RFC3339_EXTENDED (value: Y-m-d\TH:i:s.vP), which can be used to output an RFC3339 string with microseconds:<br><br><span class="default">&lt;?php<br>$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">();<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">RFC3339_EXTENDED</span><span class="keyword">)); </span><span class="comment">// 2017-07-25T13:47:12.000+00:00<br></span><span class="default">?&gt;<br></span><br>But the same constant can&apos;t be used for parsing an RFC3339 string with microseconds, instead do:<br><br><span class="default">&lt;?php<br>$date </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&quot;Y-m-d\TH:i:s.uP&quot;</span><span class="keyword">, </span><span class="string">&quot;2017-07-25T15:25:16.123456+02:00&quot;</span><span class="keyword">)<br></span><span class="default">?&gt;<br></span><br>But &quot;u&quot; can only parse microseconds up to 6 digits, but some language (like Go) return more than 6 digits for the microseconds, e.g.: &quot;2017-07-25T15:50:42.456430712+02:00&quot; (when turning time.Time to JSON with json.Marshal()). Currently there is no other solution than using a separate parsing library to get correct dates.<br><br>Note: the difference between &quot;v&quot; and &quot;u&quot; is just 3 digits vs. 6 digits.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Say if there is a string with&#xA0; $date = &quot;today is 2014 January 1&quot;;&#xA0;&#xA0; and you need to extract &quot;2014 January&quot; using DateTime::createFromFormat().&#xA0; As you can see in the string there is something odd like &quot;today is&quot; .Since that string (today is) does not correspond to a date format, we need to escape that. <br><br>In this case, each and every character on that string has to be escaped as shown below.<br><br>The code.<br><br><span class="default">&lt;?php<br>$paragraph </span><span class="keyword">= </span><span class="string">&quot;today is 2014 January 1&quot;</span><span class="keyword">;<br></span><span class="default">$date </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;\t\o\d\a\y \i\s Y F j&apos;</span><span class="keyword">, </span><span class="default">$paragraph</span><span class="keyword">);<br>echo </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y F&apos;</span><span class="keyword">); </span><span class="comment">//&quot;prints&quot; 2014 January<br><br></span><span class="keyword">- </span><span class="default">Shankar Damodaran</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
createFromFormat(&apos;U&apos;) has a strange behaviour: it ignores the datetimezone and the resulting DateTime object will always have GMT+0000 timezone.<br><br><span class="default">&lt;?php<br><br>$dt </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;U&apos;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">(), new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;CET&apos;</span><span class="keyword">));<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$dt</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s (T)&apos;</span><span class="keyword">), </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s (T)&apos;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">()));<br><br></span><span class="default">?&gt;<br></span><br>The problem is microtime() and time() returning the timestamp in current timezone.&#xA0; Instead of using time you can use &apos;now&apos; but to get a DateTimeObject with microseconds you have to write it this way to be sure to get the correct datetime:<br><br><span class="default">&lt;?php<br><br>$dt </span><span class="keyword">= \</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;U.u&apos;</span><span class="keyword">, </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">))-&gt;</span><span class="default">setTimezone</span><span class="keyword">(new \</span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;T&apos;</span><span class="keyword">)));<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Reportedly, microtime() may return a timestamp number without a fractional part if the microseconds are exactly zero.&#xA0; I.e., &quot;1463772747&quot; instead of the expected &quot;1463772747.000000&quot;.&#xA0; number_format() can create a correct string representation of the microsecond timestamp every time, which can be useful for creating DateTime objects when used with DateTime::createFromFormat():<br><br><span class="default">&lt;?php<br>$now </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;U.u&apos;</span><span class="keyword">, </span><span class="default">number_format</span><span class="keyword">(</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">), </span><span class="default">6</span><span class="keyword">, </span><span class="string">&apos;.&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">));<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$now</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s.u&apos;</span><span class="keyword">)); </span><span class="comment">// E.g., string(26) &quot;2016-05-20 19:36:26.900794&quot;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re here because you&apos;re trying to create a date from a week number, you want to be using setISODate, as I discovered here:<br><br><a href="http://www.lornajane.net/posts/2011/getting-dates-from-week-numbers-in-php" rel="nofollow" target="_blank">http://www.lornajane.net/posts/2011/getting-dates-from-week-numbers-in-php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
It can be confusing creating new DateTime from timestamp when your default timezone (date.timezone) is different from UTC and you are used to date()-function.<br><br>date()-function automatically uses your current timezone setting but DateTime::createFromFormat (or DateTime constructor) does not (it ignores tz-parameter).<br><br>You can get same results as date() by setting the timezone after object creation.<br><br><span class="default">&lt;?php<br>$ts </span><span class="keyword">= </span><span class="default">1414706400</span><span class="keyword">;<br></span><span class="default">$date1 </span><span class="keyword">= </span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;Y-m-d H:i&quot;</span><span class="keyword">, </span><span class="default">$ts</span><span class="keyword">);<br></span><span class="default">$date2 </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&quot;U&quot;</span><span class="keyword">, </span><span class="default">$ts</span><span class="keyword">)-&gt;</span><span class="default">setTimeZone</span><span class="keyword">(new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="default">date_default_timezone_get</span><span class="keyword">()))-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m-d H:i&quot;</span><span class="keyword">);<br></span><span class="comment">//$date1===$date2<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems that a pipe (&apos;|&apos;) option in formating string works only with PHP version 5.3.7 and newer. We had an issue with it on versions 5.3.2, 5.3.3, 5.3.6. Yet it was fine with 5.3.8 and 5.3.10.
<br>
<br>By short example:
<br><span class="default">&lt;?php
<br>$timezone </span><span class="keyword">= new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;UTC&apos;</span><span class="keyword">);
<br></span><span class="default">$dateTime </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;dmY|&apos;</span><span class="keyword">, </span><span class="string">&apos;01011972&apos;</span><span class="keyword">, </span><span class="default">$timezone</span><span class="keyword">);
<br></span><span class="comment">//$dateTime is FALSE in PHP v &lt;5.3.8
<br></span><span class="default">?&gt;
<br></span>
<br>Instead we used a workaround:
<br><span class="default">&lt;?php
<br>$dateTime </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;dmY&apos;</span><span class="keyword">, </span><span class="string">&apos;01011972&apos;</span><span class="keyword">, </span><span class="default">$timezone</span><span class="keyword">);
<br></span><span class="default">$dateTime</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d 00:00:00&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>which works fine.
<br>
<br>====
<br>
<br>Modified by admin to correct for version (5.3.7 not 5.3.8)</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve found that on PHP 5.5.13 (not sure if it happens on other versions) if you enter a month larger than 12 on a format that takes numeric months, the result will be a DateTime object with its month equal to the number modulo 12 instead of returning false.<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;2013-22-01&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>results in:<br>class DateTime#4 (3) {<br>&#xA0; public $date =&gt;<br>&#xA0; string(19) &quot;2014-10-01 13:05:05&quot;<br>&#xA0; public $timezone_type =&gt;<br>&#xA0; int(3)<br>&#xA0; public $timezone =&gt;<br>&#xA0; string(3) &quot;UTC&quot;<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Not a bug, but a strange issue today 2012-08-30 :<br><br><span class="default">&lt;?php<br>$date </span><span class="keyword">= </span><span class="string">&quot;2011-02&quot;</span><span class="keyword">;<br>echo </span><span class="default">$date</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">$d </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&quot;Y-m&quot;</span><span class="keyword">,</span><span class="default">$date</span><span class="keyword">);<br>echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>will display : <br>2011-02<br>2011-03<br><br>It&apos;s because there is no 2011-02-30, so datetime will take march insteed of february ...<br><br>To fix it :<br><span class="default">&lt;?php <br>$date </span><span class="keyword">= </span><span class="string">&quot;2011-02&quot;</span><span class="keyword">;<br>echo </span><span class="default">$date</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">$d </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">,</span><span class="default">$date</span><span class="keyword">.</span><span class="string">&quot;-01&quot;</span><span class="keyword">);<br>echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Y-m&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.createfromformat.php)

**[To root](/README.md)**