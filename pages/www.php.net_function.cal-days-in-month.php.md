# cal_days_in_month




<div class="phpcode"><span class="html">
Remember if you just want the days in the current month, use the date function:<br>$days = date(&quot;t&quot;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a one-line function I just wrote to find the numbers of days in a month that doesn&apos;t depend on any other functions.
<br>
<br>The reason I made this is because I just found out I forgot to compile PHP with support for calendars, and a class I&apos;m writing for my website&apos;s open source section was broken. So rather than recompiling PHP (which I will get around to tomorrow I guess), I just wrote this function which should work just as well, and will always work without the requirement of PHP&apos;s calendar extension or any other PHP functions for that matter.
<br>
<br>I learned the days of the month using the old knuckle &amp; inbetween knuckle method, so that should explain the mod 7 part. :)
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/*
<br> * days_in_month($month, $year)
<br> * Returns the number of days in a given month and year, taking into account leap years.
<br> *
<br> * $month: numeric month (integers 1-12)
<br> * $year: numeric year (any integer)
<br> *
<br> * Prec: $month is an integer between 1 and 12, inclusive, and $year is an integer.
<br> * Post: none
<br> */
<br>// corrected by ben at sparkyb dot net
<br></span><span class="keyword">function </span><span class="default">days_in_month</span><span class="keyword">(</span><span class="default">$month</span><span class="keyword">, </span><span class="default">$year</span><span class="keyword">)
<br>{
<br></span><span class="comment">// calculate number of days in a month
<br></span><span class="keyword">return </span><span class="default">$month </span><span class="keyword">== </span><span class="default">2 </span><span class="keyword">? (</span><span class="default">$year </span><span class="keyword">% </span><span class="default">4 </span><span class="keyword">? </span><span class="default">28 </span><span class="keyword">: (</span><span class="default">$year </span><span class="keyword">% </span><span class="default">100 </span><span class="keyword">? </span><span class="default">29 </span><span class="keyword">: (</span><span class="default">$year </span><span class="keyword">% </span><span class="default">400 </span><span class="keyword">? </span><span class="default">28 </span><span class="keyword">: </span><span class="default">29</span><span class="keyword">))) : ((</span><span class="default">$month </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">) % </span><span class="default">7 </span><span class="keyword">% </span><span class="default">2 </span><span class="keyword">? </span><span class="default">30 </span><span class="keyword">: </span><span class="default">31</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Enjoy,
<br>David Bindel</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.cal-days-in-month.php)

**[To root](/README.md)**