# DateTimeZone::listIdentifierstimezone_identifiers_list




<div class="phpcode"><span class="html">
Even though the manual currently says that the first parameter has to be &quot;One of DateTimeZone class constants&quot;, you may actually combine these constants:<br><br><span class="default">&lt;?php<br>&#xA0; $a </span><span class="keyword">= </span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">listIdentifiers</span><span class="keyword">(</span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">AFRICA</span><span class="keyword">); </span><span class="comment">//gives africa time zones<br>&#xA0; </span><span class="default">$b </span><span class="keyword">= </span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">listIdentifiers</span><span class="keyword">(</span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">AMERICA</span><span class="keyword">); </span><span class="comment">//gives american time zones<br>&#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">listIdentifiers</span><span class="keyword">(</span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">AFRICA </span><span class="keyword">| </span><span class="default">DateTimeZone</span><span class="keyword">::</span><span class="default">AMERICA</span><span class="keyword">); </span><span class="comment">//gives both african and american time zones<br></span><span class="default">?&gt;<br></span><br>Be sure to use |, not ||.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetimezone.listidentifiers.php)

**[To root](/README.md)**