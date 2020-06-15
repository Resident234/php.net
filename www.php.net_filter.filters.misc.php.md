# Other filters




<div class="phpcode"><span class="html">
Here is an example, since I cannot find one through out php.net&quot;<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * Strip whitespace (or other non-printable characters) from the beginning and end of a string<br> * @param string $value<br> */<br></span><span class="keyword">function </span><span class="default">trimString</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);<br>}<br><br></span><span class="default">$loginname </span><span class="keyword">= </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_POST</span><span class="keyword">, </span><span class="string">&apos;loginname&apos;</span><span class="keyword">, </span><span class="default">FILTER_CALLBACK</span><span class="keyword">, array(</span><span class="string">&apos;options&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;trimString&apos;</span><span class="keyword">));</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.misc.php)

**[To root](/README.md)**