# timezone_abbreviations_list




<div class="phpcode"><span class="html">
This was driving me nuts so I&apos;m adding a note here.
<br>
<br>Took a while to get the simple time zone abbreviation for a given time zone. If you have the name just do this:
<br>
<br><span class="default">&lt;?php
<br>$dateTime </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">();
<br></span><span class="default">$dateTime</span><span class="keyword">-&gt;</span><span class="default">setTimeZone</span><span class="keyword">(new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;America/Los_Angeles&apos;</span><span class="keyword">));
<br>return </span><span class="default">$dateTime</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;T&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Will return PST
<br>
<br>[red.: Or PDT is the current date/time is during Daylight Savings Time]</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.timezone-abbreviations-list.php)

**[To root](/README.md)**