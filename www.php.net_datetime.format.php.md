# DateTime::format




<div class="phpcode"><span class="html">
Using a datetime field from a mysql database e.g. &quot;2012-03-24 17:45:12&quot;<br><br><span class="default">&lt;?php<br><br>$result </span><span class="keyword">= </span><span class="default">mysql_query</span><span class="keyword">(</span><span class="string">&quot;SELECT `datetime` FROM `table`&quot;</span><span class="keyword">);<br></span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysql_fetch_row</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br></span><span class="default">$date </span><span class="keyword">= </span><span class="default">date_create</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);<br><br>echo </span><span class="default">date_format</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">, </span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">);<br></span><span class="comment">#output: 2012-03-24 17:45:12<br><br></span><span class="keyword">echo </span><span class="default">date_format</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">, </span><span class="string">&apos;d/m/Y H:i:s&apos;</span><span class="keyword">);<br></span><span class="comment">#output: 24/03/2012 17:45:12<br><br></span><span class="keyword">echo </span><span class="default">date_format</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">, </span><span class="string">&apos;d/m/y&apos;</span><span class="keyword">);<br></span><span class="comment">#output: 24/03/12<br><br></span><span class="keyword">echo </span><span class="default">date_format</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">, </span><span class="string">&apos;g:i A&apos;</span><span class="keyword">);<br></span><span class="comment">#output: 5:45 PM<br><br></span><span class="keyword">echo </span><span class="default">date_format</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">, </span><span class="string">&apos;G:ia&apos;</span><span class="keyword">);<br></span><span class="comment">#output: 05:45pm<br><br></span><span class="keyword">echo </span><span class="default">date_format</span><span class="keyword">(</span><span class="default">$date</span><span class="keyword">, </span><span class="string">&apos;g:ia \o\n l jS F Y&apos;</span><span class="keyword">);<br></span><span class="comment">#output: 5:45pm on Saturday 24th March 2012<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For full reference of the supported format character and results,<br>see the documentation of date() :<br><a href="http://www.php.net/manual/en/function.date.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.date.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Seems like datetime::format does not really support microseconds as the documentation under date suggest it will.<br><br>Here is some code to generate a datetime with microseconds and timezone:<br><br>private function udate($format = &apos;u&apos;, $utimestamp = null) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (is_null($utimestamp))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $utimestamp = microtime(true);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $timestamp = floor($utimestamp);<br>&#xA0; &#xA0; &#xA0; &#xA0; $milliseconds = round(($utimestamp - $timestamp) * 1000000);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return date(preg_replace(&apos;`(?&lt;!\\\\)u`&apos;, $milliseconds, $format), $timestamp);<br>&#xA0; &#xA0; }<br><br>echo udate(&apos;Y-m-d H:i:s.u T&apos;);<br>// Will output something like: 2014-01-01 12:20:24.42342 CET</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.format.php)

**[To root](/README.md)**