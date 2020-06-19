# Supported Date and Time Formats




<div class="phpcode"><span class="html">
When you&apos;ve got external inputs that do not strictly follow the formatting and disambiguation rules, you may still be able to use the static method ::createFromFormat() to create a usable DateTime object<br><br><span class="default">&lt;?php <br></span><span class="comment">/**<br> * Date values separated by slash are assumed to be in American order: m/d/y<br> * Date values separated by dash are assumed to be in European order: d-m-y<br> * Exact formats for date/time strings can be injected with ::createFromFormat()<br> */<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);<br><br></span><span class="comment">// THIS IS INVALID, WOULD IMPLY MONTH == 19<br></span><span class="default">$external </span><span class="keyword">= </span><span class="string">&quot;19/10/2016 14:48:21&quot;</span><span class="keyword">;<br><br></span><span class="comment">// HOWEVER WE CAN INJECT THE FORMATTING WHEN WE DECODE THE DATE<br></span><span class="default">$format </span><span class="keyword">= </span><span class="string">&quot;d/m/Y H:i:s&quot;</span><span class="keyword">;<br></span><span class="default">$dateobj </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">, </span><span class="default">$external</span><span class="keyword">);<br><br></span><span class="default">$iso_datetime </span><span class="keyword">= </span><span class="default">$dateobj</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">Datetime</span><span class="keyword">::</span><span class="default">ATOM</span><span class="keyword">);<br>echo </span><span class="string">&quot;SUCCESS: </span><span class="default">$external</span><span class="string"> EQUALS ISO-8601 </span><span class="default">$iso_datetime</span><span class="string">&quot;</span><span class="keyword">;<br><br></span><span class="comment">// MAN PAGE: <a href="http://php.net/manual/en/datetime.createfromformat.php" rel="nofollow" target="_blank">http://php.net/manual/en/datetime.createfromformat.php</a></span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.formats.php)

**[To root](/README.md)**