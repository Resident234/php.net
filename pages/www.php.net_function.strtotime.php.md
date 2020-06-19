# strtotime




<div class="phpcode"><span class="html">
I&apos;ve had a little trouble with this function in the past because (as some people have pointed out) you can&apos;t really set a locale for strtotime. If you&apos;re American, you see 11/12/10 and think &quot;12 November, 2010&quot;. If you&apos;re Australian (or European), you think it&apos;s 11 December, 2010. If you&apos;re a sysadmin who reads in ISO, it looks like 10th December 2011.
<br>
<br>The best way to compensate for this is by modifying your joining characters. Forward slash (/) signifies American M/D/Y formatting, a dash (-) signifies European D-M-Y and a period (.) signifies ISO Y.M.D.
<br>
<br>Observe:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">echo </span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;jS F, Y&quot;</span><span class="keyword">, </span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&quot;11.12.10&quot;</span><span class="keyword">));
<br></span><span class="comment">// outputs 10th December, 2011
<br>
<br></span><span class="keyword">echo </span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;jS F, Y&quot;</span><span class="keyword">, </span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&quot;11/12/10&quot;</span><span class="keyword">));
<br></span><span class="comment">// outputs 12th November, 2010
<br>
<br></span><span class="keyword">echo </span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;jS F, Y&quot;</span><span class="keyword">, </span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&quot;11-12-10&quot;</span><span class="keyword">));
<br></span><span class="comment">// outputs 11th December, 2010&#xA0; 
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this helps someone!</span>
</div>
  

#


<div class="phpcode"><span class="html">
The &quot;+1 month&quot; issue with strtotime<br>===================================<br>As noted in several blogs, strtotime() solves the &quot;+1 month&quot; (&quot;next month&quot;) issue on days that do not exist in the subsequent month differently than other implementations like for example MySQL.<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">date</span><span class="keyword">( </span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">, </span><span class="default">strtotime</span><span class="keyword">( </span><span class="string">&quot;2009-01-31 +1 month&quot; </span><span class="keyword">) ); </span><span class="comment">// PHP:&#xA0; 2009-03-03<br></span><span class="keyword">echo </span><span class="default">date</span><span class="keyword">( </span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">, </span><span class="default">strtotime</span><span class="keyword">( </span><span class="string">&quot;2009-01-31 +2 month&quot; </span><span class="keyword">) ); </span><span class="comment">// PHP:&#xA0; 2009-03-31<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br>SELECT DATE_ADD</span><span class="keyword">( </span><span class="string">&apos;2009-01-31&apos;</span><span class="keyword">, </span><span class="default">INTERVAL 1 MONTH </span><span class="keyword">); </span><span class="comment">// MySQL:&#xA0; 2009-02-28<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
UK dates (eg. 27/05/1990) won&apos;t work with strotime, even with timezone properly set. 
<br>
<br>/*
<br>However, if you just replace &quot;/&quot; with &quot;-&quot; it will work fine.
<br><span class="default">&lt;?php
<br>$timestamp </span><span class="keyword">= </span><span class="default">strtotime</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="string">&apos;27/05/1990&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>*/
<br>
<br>[red., derick]: What you instead should do is:
<br>
<br><span class="default">&lt;?php
<br>$date </span><span class="keyword">= </span><span class="default">date_create_from_format</span><span class="keyword">(</span><span class="string">&apos;d/m/y&apos;</span><span class="keyword">, </span><span class="string">&apos;27/05/1990&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>That does not make it a timestamp, but a DateTime object, which is much more versatile instead.</span>
</div>
  

#


<div class="phpcode"><span class="html">
WARNING when using &quot;next month&quot;, &quot;last month&quot;, &quot;+1 month&quot;,&#xA0; &quot;-1 month&quot; or any combination of +/-X months. It will give non-intuitive results on Jan 30th and 31st. <br><br>As described at : <a href="http://derickrethans.nl/obtaining-the-next-month-in-php.html" rel="nofollow" target="_blank">http://derickrethans.nl/obtaining-the-next-month-in-php.html</a><br><br><span class="default">&lt;?php<br>$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2010-01-31&apos; </span><span class="keyword">);<br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">( </span><span class="string">&apos;next month&apos; </span><span class="keyword">);<br>echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;F&apos; </span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>In the above, using &quot;next month&quot; on January 31 will output &quot;March&quot; even though you might want it to output &quot;February&quot;. (&quot;+1 month&quot; will give the same result. &quot;last month&quot;, &quot;-1 month&quot; are similarly affected, but the results would be seen at beginning of March.)<br><br>The way to get what people would generally be looking for when they say &quot;next month&quot; even on Jan 30 and Jan 31 is to use &quot;first day of next month&quot;:<br><br><span class="default">&lt;?php<br>$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2010-01-08&apos; </span><span class="keyword">);<br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">( </span><span class="string">&apos;first day of next month&apos; </span><span class="keyword">);<br>echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;F&apos; </span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br>$d </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2010-01-08&apos; </span><span class="keyword">);<br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">( </span><span class="string">&apos;first day of +1 month&apos; </span><span class="keyword">);<br>echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&apos;F&apos; </span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A useful testing tool for strtotime() and unix timestamp conversion:<br><a href="http://strtotime.co.uk/" rel="nofollow" target="_blank">http://strtotime.co.uk/</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
strtotime() also returns time by year and weeknumber. (I use PHP 5.2.8, PHP 4 does not support it.) Queries can be in two forms:
<br>- &quot;yyyyWww&quot;, where yyyy is 4-digit year, W is literal and ww is 2-digit weeknumber. Returns timestamp for first day of week (for me Monday)
<br>- &quot;yyyy-Www-d&quot;, where yyyy is 4-digit year, W is literal, ww is 2-digit weeknumber and dd is day of week (1 for Monday, 7 for Sunday) 
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// Get timestamp of 32nd week in 2009.
<br></span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&apos;2009W32&apos;</span><span class="keyword">); </span><span class="comment">// returns timestamp for Mon, 03 Aug 2009 00:00:00
<br>// Weeknumbers &lt; 10 must be padded with zero:
<br></span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&apos;2009W01&apos;</span><span class="keyword">); </span><span class="comment">// returns timestamp for Mon, 29 Dec 2008 00:00:00
<br>// strtotime(&apos;2009W1&apos;); // error! returns false
<br>
<br>// See timestamp for Tuesday in 5th week of 2008
<br></span><span class="default">strtotime</span><span class="keyword">(</span><span class="string">&apos;2008-W05-2&apos;</span><span class="keyword">); </span><span class="comment">// returns timestamp for Tue, 29 Jan 2008 00:00:00
<br></span><span class="default">?&gt;
<br></span>
<br>Weeknumbers are (probably) computed according to ISO-8601 specification, so doing date(&apos;W&apos;) on given timestamps should return passed weeknumber.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strtotime.php)

**[To root](/README.md)**