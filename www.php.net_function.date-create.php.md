# date_create




<div class="phpcode"><span class="html">
If you want to create the DateTime object directly from a timestamp use this
<br>
<br><span class="default">&lt;?php
<br>$st </span><span class="keyword">= </span><span class="default">1170288000 </span><span class="comment">//&#xA0; a timestamp
<br></span><span class="default">$dt </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;@</span><span class="default">$st</span><span class="string">&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>See also: <a href="http://bugs.php.net/bug.php?id=40171" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=40171</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are getting an error like this:<br><br>Exception: DateTime::__construct(): Failed to parse time string (13/02/2013) at position 0 (1): Unexpected character in DateTime-&gt;__construct()<br><br>Note that when you create a new date object using a format with slashes and dashes (eg 02-02-2012 or 02/02/2012) it must be in the mm/dd/yy(yy) or mm-dd-yy(yy) format (rather than british format dd/mm/yy)! Months always before years (the american style) otherwise you&apos;ll get an incorrect date and may get an error like the one above (where PHP is crashing on trying to decode a 13th month).<br><br>Can catch you off guard because everything seems to be working fine and dandy until you hit a value over 12.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.date-create.php)

**[To root](/README.md)**