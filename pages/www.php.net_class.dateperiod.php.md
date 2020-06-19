# The DatePeriod class




<div class="phpcode"><span class="html">
Just an example to include the end date using the DateTime method &apos;modify&apos;<br><br><span class="default">&lt;?php<br><br>$begin </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2012-08-01&apos; </span><span class="keyword">);<br></span><span class="default">$end </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2012-08-31&apos; </span><span class="keyword">);<br></span><span class="default">$end </span><span class="keyword">= </span><span class="default">$end</span><span class="keyword">-&gt;</span><span class="default">modify</span><span class="keyword">( </span><span class="string">&apos;+1 day&apos; </span><span class="keyword">); <br><br></span><span class="default">$interval </span><span class="keyword">= new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&apos;P1D&apos;</span><span class="keyword">);<br></span><span class="default">$daterange </span><span class="keyword">= new </span><span class="default">DatePeriod</span><span class="keyword">(</span><span class="default">$begin</span><span class="keyword">, </span><span class="default">$interval </span><span class="keyword">,</span><span class="default">$end</span><span class="keyword">);<br><br>foreach(</span><span class="default">$daterange </span><span class="keyword">as </span><span class="default">$date</span><span class="keyword">){<br>&#xA0; &#xA0; echo </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;Ymd&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Thanks much to those of you who supplied sample code; that helps a lot.<br><br>I wanted to mention another thing that helped me: when you do that foreach ( $period as $dt ), the $dt values are DateTime objects.<br><br>That may be obvious to those of you with more experience, but I wasn&apos;t sure until I looked it up on Stack Overflow. So I figured it was worth posting here to help others like me who might&apos;ve been confused or uncertain.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Nice example from PHP Spring Conference (thanks to Johannes Schl&#xFC;ter and David Z&#xFC;lke)<br><br><span class="default">&lt;?php<br>$begin </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2007-12-31&apos; </span><span class="keyword">);<br></span><span class="default">$end </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">( </span><span class="string">&apos;2009-12-31 23:59:59&apos; </span><span class="keyword">);<br><br></span><span class="default">$interval </span><span class="keyword">= </span><span class="default">DateInterval</span><span class="keyword">::</span><span class="default">createFromDateString</span><span class="keyword">(</span><span class="string">&apos;last thursday of next month&apos;</span><span class="keyword">);<br></span><span class="default">$period </span><span class="keyword">= new </span><span class="default">DatePeriod</span><span class="keyword">(</span><span class="default">$begin</span><span class="keyword">, </span><span class="default">$interval</span><span class="keyword">, </span><span class="default">$end</span><span class="keyword">, </span><span class="default">DatePeriod</span><span class="keyword">::</span><span class="default">EXCLUDE_START_DATE</span><span class="keyword">);<br><br>foreach ( </span><span class="default">$period </span><span class="keyword">as </span><span class="default">$dt </span><span class="keyword">)<br>&#xA0; echo </span><span class="default">$dt</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">( </span><span class="string">&quot;l Y-m-d H:i:s\n&quot; </span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>DateInterval specs could be found at <a href="http://en.wikipedia.org/wiki/ISO_8601#Time_intervals" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/ISO_8601#Time_intervals</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.dateperiod.php)

**[To root](/README.md)**