# date_format




<div class="phpcode"><span class="html">
Requirements: PHP5+
<br>
<br>To expand on Matt Walsh&apos;s example, a simple way to get eBay, or Amazon, web service timestamps is as follows:
<br>
<br><span class="default">&lt;?php
<br>
<br>$current_time </span><span class="keyword">= </span><span class="default">urlEncode</span><span class="keyword">(</span><span class="default">subStr</span><span class="keyword">(</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;c&quot;</span><span class="keyword">), </span><span class="default">0</span><span class="keyword">, </span><span class="default">19</span><span class="keyword">).</span><span class="string">&quot;Z&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>In other words, take the date/time of now (in ISO 8601 format), discard the trailing Daylight Savings Time specifier, add a &quot;Z&quot; where the DST was and urlEncode the whole thing to convert the time&apos;s colons for REST requests (required for amazon, not sure about eBay).
<br>
<br>Another way might be to create your own timestamp:
<br>
<br><span class="default">&lt;?php
<br>
<br>$current_time </span><span class="keyword">= </span><span class="default">urlEncode</span><span class="keyword">(</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">).</span><span class="string">&quot;T&quot;</span><span class="keyword">.</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;H:i:s&quot;</span><span class="keyword">).</span><span class="string">&quot;Z&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>This way however takes a little more coding on the line.
<br>
<br>As far as performance goes, I&apos;m not sure which may be quicker. I just like things to work and work well, don&apos;t much care for how fast they are as long as they get the job done :)
<br>
<br>A much simpler way to get the eBay, or Amazon, web service timestamp is as follows:
<br>
<br><span class="default">&lt;?php
<br>
<br>$current_date </span><span class="keyword">= </span><span class="default">gmDate</span><span class="keyword">(</span><span class="string">&quot;Y-m-d\TH:i:s\Z&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Enjoy!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.date-format.php)

**[To root](/README.md)**