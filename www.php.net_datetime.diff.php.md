# DateTime::diffDateTimeImmutable::diffDateTimeInterface::diffdate_diff




<div class="phpcode"><span class="html">
It is worth noting, IMO, and it is implied in the docs but not explicitly stated, that the object on which diff is called is subtracted from the object that is passed to diff.<br><br>i.e. $now-&gt;diff($tomorrow) is positive.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful using:<br><br>$date1 = new DateTime(&apos;now&apos;);<br>$date2 = new DateTime(&apos;tomorrow&apos;);<br><br>$interval = date_diff($date1, $date2);<br><br>echo $interval-&gt;format(&apos;In %a days&apos;);<br><br>In some situations, this won&apos;t say &quot;in 1 days&quot;, but &quot;in 0 days&quot;.<br>I think this is because &quot;now&quot; is the current time, while &quot;tomorrow&quot; is the current day +1 but at a default time, lets say:<br><br>Now: 08:00pm, 01.01.2015<br>Tomorrow: 00:00am, 02.01.2015<br><br>In this case, the difference is not 24 hour, so it will says 0 days.<br><br>Better use &quot;today&quot;, which should also use a default value like:<br><br>Today: 00:00am, 01.01.2015<br>Tomorrow: 00:00am, 02.01.2015<br><br>which now is 24 hour and represents 1 day.<br><br>This may sound logical and many will say &quot;of course, this is right&quot;, but if you use it in a naiv way (like I did without thinking), you can come to this moment and facepalm yourself.<br><br>Conclusion: &quot;Now&quot; is &quot;Today&quot;, but in a different clock time, but still the same day!</span>
</div>
  

#


<div class="phpcode"><span class="html">
After wrestling with DateTime::diff for a while it finally dawned on me the problem was both in the formatting of the input string and the formatting of the output.
<br>
<br>The task was to calculate the duration between two date/times.
<br>
<br>### Calculating Duration 
<br>
<br>1. Make sure you have a valid date variable.&#xA0; Both of these strings are valid:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">// Example
<br>
<br>&#xA0;&#xA0; </span><span class="default">$strStart </span><span class="keyword">= </span><span class="string">&apos;2013-06-19 18:25&apos;</span><span class="keyword">;
<br>&#xA0;&#xA0; </span><span class="default">$strEnd&#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&apos;06/19/13 21:47&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>2. Next convert the string to a date variable
<br>~~~
<br><span class="default">&lt;?php
<br>
<br>&#xA0;&#xA0; $dteStart </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$strStart</span><span class="keyword">);
<br>&#xA0;&#xA0; </span><span class="default">$dteEnd&#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$strEnd</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>~~~
<br>
<br>3. Calculate the difference
<br>~~~
<br><span class="default">&lt;?php
<br>
<br>&#xA0;&#xA0; $dteDiff&#xA0; </span><span class="keyword">= </span><span class="default">$dteStart</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$dteEnd</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>~~~
<br>
<br>4. Format the output
<br>~~~
<br><span class="default">&lt;?php
<br>
<br>&#xA0;&#xA0; </span><span class="keyword">print </span><span class="default">$dteDiff</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;%H:%I:%S&quot;</span><span class="keyword">);
<br>
<br></span><span class="comment">/*
<br>&#xA0; &#xA0; Outputs
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; 03:22:00
<br>*/
<br>
<br></span><span class="default">?&gt;
<br></span>~~~
<br>
<br>[Modified by moderator for clarify]</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using the identical (===) comparision operator in different but equal objects will return false<br><br><span class="default">&lt;?php<br>$c </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2014-04-20&apos; </span><span class="keyword">);<br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2014-04-20&apos; </span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">( </span><span class="default">$d </span><span class="keyword">=== </span><span class="default">$d </span><span class="keyword">); </span><span class="comment">#true<br></span><span class="default">var_dump</span><span class="keyword">( </span><span class="default">$d </span><span class="keyword">=== </span><span class="default">$c </span><span class="keyword">); </span><span class="comment">#false<br></span><span class="default">var_dump</span><span class="keyword">( </span><span class="default">$d </span><span class="keyword">== </span><span class="default">$c </span><span class="keyword">); </span><span class="comment">#true<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to quickly scan through the resulting intervals, you can use the undocumented properties of DateInterval.<br><br>The function below returns a single number of years, months, days, hours, minutes or seconds between the current date and the provided date.&#xA0; If the date occurs in the past (is negative/inverted), it suffixes it with &apos;ago&apos;.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$count</span><span class="keyword">, </span><span class="default">$text </span><span class="keyword">) <br>{ <br>&#xA0; &#xA0; return </span><span class="default">$count </span><span class="keyword">. ( ( </span><span class="default">$count </span><span class="keyword">== </span><span class="default">1 </span><span class="keyword">) ? ( </span><span class="string">&quot; </span><span class="default">$text</span><span class="string">&quot; </span><span class="keyword">) : ( </span><span class="string">&quot; </span><span class="keyword">${</span><span class="default">text</span><span class="keyword">}</span><span class="string">s&quot; </span><span class="keyword">) );<br>}<br><br>function </span><span class="default">ago</span><span class="keyword">( </span><span class="default">$datetime </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$interval </span><span class="keyword">= </span><span class="default">date_create</span><span class="keyword">(</span><span class="string">&apos;now&apos;</span><span class="keyword">)-&gt;</span><span class="default">diff</span><span class="keyword">( </span><span class="default">$datetime </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$suffix </span><span class="keyword">= ( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">invert </span><span class="keyword">? </span><span class="string">&apos; ago&apos; </span><span class="keyword">: </span><span class="string">&apos;&apos; </span><span class="keyword">);<br>&#xA0; &#xA0; if ( </span><span class="default">$v </span><span class="keyword">= </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">) return </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">y</span><span class="keyword">, </span><span class="string">&apos;year&apos; </span><span class="keyword">) . </span><span class="default">$suffix</span><span class="keyword">;<br>&#xA0; &#xA0; if ( </span><span class="default">$v </span><span class="keyword">= </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">) return </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">m</span><span class="keyword">, </span><span class="string">&apos;month&apos; </span><span class="keyword">) . </span><span class="default">$suffix</span><span class="keyword">;<br>&#xA0; &#xA0; if ( </span><span class="default">$v </span><span class="keyword">= </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">d </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">) return </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">d</span><span class="keyword">, </span><span class="string">&apos;day&apos; </span><span class="keyword">) . </span><span class="default">$suffix</span><span class="keyword">;<br>&#xA0; &#xA0; if ( </span><span class="default">$v </span><span class="keyword">= </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">h </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">) return </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">h</span><span class="keyword">, </span><span class="string">&apos;hour&apos; </span><span class="keyword">) . </span><span class="default">$suffix</span><span class="keyword">;<br>&#xA0; &#xA0; if ( </span><span class="default">$v </span><span class="keyword">= </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">i </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">) return </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">i</span><span class="keyword">, </span><span class="string">&apos;minute&apos; </span><span class="keyword">) . </span><span class="default">$suffix</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">s</span><span class="keyword">, </span><span class="string">&apos;second&apos; </span><span class="keyword">) . </span><span class="default">$suffix</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems that while DateTime in general does preserve microseconds, DateTime::diff doesn&apos;t appear to account for it when comparing.&#xA0; <br><br>Example:<br><br><span class="default">&lt;?php<br>$val1 </span><span class="keyword">= </span><span class="string">&apos;2014-03-18 10:34:09.939&apos;</span><span class="keyword">;<br></span><span class="default">$val2 </span><span class="keyword">= </span><span class="string">&apos;2014-03-18 10:34:09.940&apos;</span><span class="keyword">;<br><br></span><span class="default">$datetime1 </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$val1</span><span class="keyword">);<br></span><span class="default">$datetime2 </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$val2</span><span class="keyword">);<br>echo </span><span class="string">&quot;&lt;pre&gt;&quot;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$datetime1</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$datetime2</span><span class="keyword">));<br><br>if(</span><span class="default">$datetime1 </span><span class="keyword">&gt; </span><span class="default">$datetime2</span><span class="keyword">)<br>&#xA0; echo </span><span class="string">&quot;1 is bigger&quot;</span><span class="keyword">;<br>else<br>&#xA0; echo </span><span class="string">&quot;2 is bigger&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>The var_dump shows that there is no &quot;u&quot; element, and &quot;2 is bigger&quot; is echoed. <br><br>To work around this apparent limitation/oversight, you have to additionally compare using DateTime::format.&#xA0; <br><br>Example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if(</span><span class="default">$datetime1 </span><span class="keyword">&gt; </span><span class="default">$datetime2</span><span class="keyword">)<br>&#xA0; echo </span><span class="string">&quot;1 is bigger&quot;</span><span class="keyword">;<br>else if (</span><span class="default">$datetime1</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;u&apos;</span><span class="keyword">) &gt; </span><span class="default">$datetime2</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;u&apos;</span><span class="keyword">))<br>&#xA0; echo </span><span class="string">&quot;1 is bigger&quot;</span><span class="keyword">;<br>else<br>&#xA0; echo </span><span class="string">&quot;2 is bigger&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Warning, there&apos;s a bug on windows platforms: the result is always 6015 days (and not 42...)<br><br><a href="http://bugs.php.net/bug.php?id=51184" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=51184</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Though I found a number of people who ran into the issue of 5.2 and lower not supporting this function, I was unable to find any solid examples to get around it. Therefore I hope this can help some others:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">get_timespan_string</span><span class="keyword">(</span><span class="default">$older</span><span class="keyword">, </span><span class="default">$newer</span><span class="keyword">) {
<br>&#xA0; </span><span class="default">$Y1 </span><span class="keyword">= </span><span class="default">$older</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$Y2 </span><span class="keyword">= </span><span class="default">$newer</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$Y </span><span class="keyword">= </span><span class="default">$Y2 </span><span class="keyword">- </span><span class="default">$Y1</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="default">$m1 </span><span class="keyword">= </span><span class="default">$older</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;m&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$m2 </span><span class="keyword">= </span><span class="default">$newer</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;m&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$m </span><span class="keyword">= </span><span class="default">$m2 </span><span class="keyword">- </span><span class="default">$m1</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="default">$d1 </span><span class="keyword">= </span><span class="default">$older</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;d&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$d2 </span><span class="keyword">= </span><span class="default">$newer</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;d&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$d </span><span class="keyword">= </span><span class="default">$d2 </span><span class="keyword">- </span><span class="default">$d1</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="default">$H1 </span><span class="keyword">= </span><span class="default">$older</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;H&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$H2 </span><span class="keyword">= </span><span class="default">$newer</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;H&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$H </span><span class="keyword">= </span><span class="default">$H2 </span><span class="keyword">- </span><span class="default">$H1</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="default">$i1 </span><span class="keyword">= </span><span class="default">$older</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;i&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$i2 </span><span class="keyword">= </span><span class="default">$newer</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;i&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$i </span><span class="keyword">= </span><span class="default">$i2 </span><span class="keyword">- </span><span class="default">$i1</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="default">$s1 </span><span class="keyword">= </span><span class="default">$older</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;s&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$s2 </span><span class="keyword">= </span><span class="default">$newer</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;s&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$s </span><span class="keyword">= </span><span class="default">$s2 </span><span class="keyword">- </span><span class="default">$s1</span><span class="keyword">;
<br>
<br>&#xA0; if(</span><span class="default">$s </span><span class="keyword">&lt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$i </span><span class="keyword">= </span><span class="default">$i </span><span class="keyword">-</span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$s </span><span class="keyword">= </span><span class="default">$s </span><span class="keyword">+ </span><span class="default">60</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$H </span><span class="keyword">= </span><span class="default">$H </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$i </span><span class="keyword">= </span><span class="default">$i </span><span class="keyword">+ </span><span class="default">60</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$H </span><span class="keyword">&lt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$d </span><span class="keyword">= </span><span class="default">$d </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$H </span><span class="keyword">= </span><span class="default">$H </span><span class="keyword">+ </span><span class="default">24</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$d </span><span class="keyword">&lt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$m </span><span class="keyword">= </span><span class="default">$m </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$d </span><span class="keyword">= </span><span class="default">$d </span><span class="keyword">+ </span><span class="default">get_days_for_previous_month</span><span class="keyword">(</span><span class="default">$m2</span><span class="keyword">, </span><span class="default">$Y2</span><span class="keyword">);
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$m </span><span class="keyword">&lt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$Y </span><span class="keyword">= </span><span class="default">$Y </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$m </span><span class="keyword">= </span><span class="default">$m </span><span class="keyword">+ </span><span class="default">12</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; </span><span class="default">$timespan_string </span><span class="keyword">= </span><span class="default">create_timespan_string</span><span class="keyword">(</span><span class="default">$Y</span><span class="keyword">, </span><span class="default">$m</span><span class="keyword">, </span><span class="default">$d</span><span class="keyword">, </span><span class="default">$H</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="default">$s</span><span class="keyword">);
<br>&#xA0; return </span><span class="default">$timespan_string</span><span class="keyword">;
<br>}
<br>
<br>function </span><span class="default">get_days_for_previous_month</span><span class="keyword">(</span><span class="default">$current_month</span><span class="keyword">, </span><span class="default">$current_year</span><span class="keyword">) {
<br>&#xA0; </span><span class="default">$previous_month </span><span class="keyword">= </span><span class="default">$current_month </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; if(</span><span class="default">$current_month </span><span class="keyword">== </span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$current_year </span><span class="keyword">= </span><span class="default">$current_year </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">; </span><span class="comment">//going from January to previous December
<br>&#xA0; &#xA0; </span><span class="default">$previous_month </span><span class="keyword">= </span><span class="default">12</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$previous_month </span><span class="keyword">== </span><span class="default">11 </span><span class="keyword">|| </span><span class="default">$previous_month </span><span class="keyword">== </span><span class="default">9 </span><span class="keyword">|| </span><span class="default">$previous_month </span><span class="keyword">== </span><span class="default">6 </span><span class="keyword">|| </span><span class="default">$previous_month </span><span class="keyword">== </span><span class="default">4</span><span class="keyword">) {
<br>&#xA0; &#xA0; return </span><span class="default">30</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; else if(</span><span class="default">$previous_month </span><span class="keyword">== </span><span class="default">2</span><span class="keyword">) {
<br>&#xA0; &#xA0; if((</span><span class="default">$current_year </span><span class="keyword">% </span><span class="default">4</span><span class="keyword">) == </span><span class="default">0</span><span class="keyword">) { </span><span class="comment">//remainder 0 for leap years
<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">29</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">28</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; }
<br>&#xA0; else {
<br>&#xA0; &#xA0; return </span><span class="default">31</span><span class="keyword">;
<br>&#xA0; }
<br>}
<br>
<br>function </span><span class="default">create_timespan_string</span><span class="keyword">(</span><span class="default">$Y</span><span class="keyword">, </span><span class="default">$m</span><span class="keyword">, </span><span class="default">$d</span><span class="keyword">, </span><span class="default">$H</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="default">$s</span><span class="keyword">)
<br>{
<br>&#xA0; </span><span class="default">$timespan_string </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$found_first_diff </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; if(</span><span class="default">$Y </span><span class="keyword">&gt;= </span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$found_first_diff </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="default">pluralize</span><span class="keyword">(</span><span class="default">$Y</span><span class="keyword">, </span><span class="string">&apos;year&apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$m </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">$found_first_diff</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$found_first_diff </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="default">pluralize</span><span class="keyword">(</span><span class="default">$m</span><span class="keyword">, </span><span class="string">&apos;month&apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$d </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">$found_first_diff</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$found_first_diff </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="default">pluralize</span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">, </span><span class="string">&apos;day&apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$H </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">$found_first_diff</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$found_first_diff </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="default">pluralize</span><span class="keyword">(</span><span class="default">$H</span><span class="keyword">, </span><span class="string">&apos;hour&apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$i </span><span class="keyword">&gt;= </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">$found_first_diff</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$found_first_diff </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="default">pluralize</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">, </span><span class="string">&apos;minute&apos;</span><span class="keyword">).</span><span class="string">&apos; &apos;</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; if(</span><span class="default">$found_first_diff</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="string">&apos;and &apos;</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; </span><span class="default">$timespan_string </span><span class="keyword">.= </span><span class="default">pluralize</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">, </span><span class="string">&apos;second&apos;</span><span class="keyword">);
<br>&#xA0; return </span><span class="default">$timespan_string</span><span class="keyword">;
<br>}
<br>
<br>function </span><span class="default">pluralize</span><span class="keyword">( </span><span class="default">$count</span><span class="keyword">, </span><span class="default">$text </span><span class="keyword">)
<br>{
<br>&#xA0; return </span><span class="default">$count </span><span class="keyword">. ( ( </span><span class="default">$count </span><span class="keyword">== </span><span class="default">1 </span><span class="keyword">) ? ( </span><span class="string">&quot; </span><span class="default">$text</span><span class="string">&quot; </span><span class="keyword">) : ( </span><span class="string">&quot; </span><span class="keyword">${</span><span class="default">text</span><span class="keyword">}</span><span class="string">s&quot; </span><span class="keyword">) );
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.diff.php)

**[To root](/README.md)**