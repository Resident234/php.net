# DatePeriod::__construct




<div class="phpcode"><span class="html">
I found two things useful to know that aren&apos;t covered here.
<br>
<br>1. endDate is excluded:
<br>
<br><span class="default">&lt;?php
<br>$i </span><span class="keyword">= new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&apos;P1D&apos;</span><span class="keyword">);
<br></span><span class="default">$d1 </span><span class="keyword">= new </span><span class="default">Datetime</span><span class="keyword">();
<br></span><span class="default">$d2 </span><span class="keyword">= clone </span><span class="default">$d1</span><span class="keyword">; </span><span class="default">$d2</span><span class="keyword">-&gt;</span><span class="default">add</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">);
<br>foreach(new </span><span class="default">DatePeriod</span><span class="keyword">(</span><span class="default">$d1</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="default">$d2</span><span class="keyword">) as </span><span class="default">$d</span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Will output:
<br>2010-11-03 12:39:53
<br>
<br>(Another one because I got it wrong at first)
<br>2. For the first form, recurrences really means REcurrences, not occurences.
<br>
<br><span class="default">&lt;?php
<br>$i </span><span class="keyword">= new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&apos;P1D&apos;</span><span class="keyword">);
<br></span><span class="default">$d </span><span class="keyword">= new </span><span class="default">Datetime</span><span class="keyword">();
<br>foreach(new </span><span class="default">DatePeriod</span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">) as </span><span class="default">$d</span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Will output:
<br>2010-11-03 12:41:05
<br>2010-11-04 12:41:05</span>
</div>
  

#


<div class="phpcode"><span class="html">
When you add the time 23:59:59 to the end DateTime object something like the following then the end date will be included in the period:
<br>
<br><span class="default">&lt;?php
<br>$date_start </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;2012-03-12&apos;</span><span class="keyword">);
<br></span><span class="default">$date_end </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&apos;2012-03-22 23:59:59&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">$interval </span><span class="keyword">= </span><span class="string">&apos;+2 days&apos;</span><span class="keyword">;
<br></span><span class="default">$date_interval </span><span class="keyword">= </span><span class="default">DateInterval</span><span class="keyword">::</span><span class="default">createFromDateString</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">);
<br>
<br></span><span class="default">$period </span><span class="keyword">= new </span><span class="default">DatePeriod</span><span class="keyword">(</span><span class="default">$date_start</span><span class="keyword">, </span><span class="default">$date_interval</span><span class="keyword">, </span><span class="default">$date_end</span><span class="keyword">, </span><span class="default">DatePeriod</span><span class="keyword">::</span><span class="default">EXCLUDE_START_DATE</span><span class="keyword">);
<br>
<br>foreach(</span><span class="default">$period </span><span class="keyword">as </span><span class="default">$dt</span><span class="keyword">) {
<br> echo </span><span class="default">$dt</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;d/m&apos;</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>OUTPUT:
<br>14/03
<br>16/03
<br>18/03
<br>20/03
<br>22/03</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/dateperiod.construct.php)

**[To root](/README.md)**