# DateTime::getTimestamp




<div class="phpcode"><span class="html">
Note that for dates before the unix epoch getTimestamp() will return false, whereas format(&quot;U&quot;) will return a negative number.<br><br><span class="default">&lt;?php<br>$date </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;1899-12-31&quot;</span><span class="keyword">);<br></span><span class="comment">// &quot;-2209078800&quot;<br></span><span class="keyword">echo </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&quot;U&quot;</span><span class="keyword">);<br></span><span class="comment">// false<br></span><span class="keyword">echo </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">getTimestamp</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In 32-bit system the unix timestamp will overflow if the date goes beyond year 2038 and this method will return false. In 64-bit systems this function will still work as intended. For more information please see <a href="http://en.wikipedia.org/wiki/Year_2038_problem." rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Year_2038_problem.</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.gettimestamp.php)

**[To root](/README.md)**