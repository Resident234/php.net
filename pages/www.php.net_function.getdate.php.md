# getdate




<div class="phpcode"><span class="html">
Andre&apos;s code will throw an error. for the following line<br>&#xA0; &#xA0; <br>&#xA0; &#xA0;&#xA0; $d = $todayh[mday];<br>&#xA0; &#xA0;&#xA0; $m = $todayh[mon];<br>&#xA0; &#xA0;&#xA0; $y = $todayh[year];<br><br>&quot;Notice : Undefined constant mday ,mon,year&quot;<br><br>As is, it was looking for constants called mday, mon, year etc. When it doesn&apos;t find such a constant, PHP interprets it as a string. <br><br>like any other request it should be wrapped in quotes like this<br><br>&#xA0; &#xA0;&#xA0; $d = $todayh[&apos;mday&apos;];<br>&#xA0; &#xA0;&#xA0; $m = $todayh[&apos;mon&apos;];<br>&#xA0; &#xA0;&#xA0; $y = $todayh[&apos;year&apos;];</span>
</div>
  

#


<div class="phpcode"><span class="html">
In addition to canby23 at ms19 post:<br>It&apos;s a very bad idea to consider day having 24 hours (86400 secs), because some days have 23, some - 25 hours due to daylight saving changes. Using of mkdate() and strtotime() is always preferred. strtotime() also has a very nice behaviour of datetime manipulations:<br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">strtotime </span><span class="keyword">(</span><span class="string">&quot;+1 day&quot;</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>echo </span><span class="default">strtotime </span><span class="keyword">(</span><span class="string">&quot;+1 week&quot;</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>echo </span><span class="default">strtotime </span><span class="keyword">(</span><span class="string">&quot;+1 week 2 days 4 hours 2 seconds&quot;</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>echo </span><span class="default">strtotime </span><span class="keyword">(</span><span class="string">&quot;next Thursday&quot;</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>echo </span><span class="default">strtotime </span><span class="keyword">(</span><span class="string">&quot;last Monday&quot;</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">; <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.getdate.php)

**[To root](/README.md)**