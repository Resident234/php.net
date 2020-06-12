# Date and Time




<div class="phpcode"><span class="html">
I think it&apos;s important to mention with the DateTime class that if you&apos;re trying to create a system that should store UNIX timestamps in UTC/GMT, and then convert them to a desired custom time-zone when they need to be displayed, using the following code is a good idea:<br><br><span class="default">&lt;?php<br>date_default_timezone_set</span><span class="keyword">(</span><span class="string">&apos;UTC&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Even if you use something like:<br><br><span class="default">&lt;?php<br>$date</span><span class="keyword">-&gt;</span><span class="default">setTimezone</span><span class="keyword">( new </span><span class="default">DateTimeZone</span><span class="keyword">(</span><span class="string">&apos;UTC&apos;</span><span class="keyword">) );<br></span><span class="default">?&gt;<br></span><br>... before you store the value, it doesn&apos;t seem to work because PHP is already trying to convert it to the default timezone.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.datetime.php)

**[To root](/README.md)**