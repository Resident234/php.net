# date_parse




<div class="phpcode"><span class="html">
A warning to others. Some keys will return with a default value where others will return as false if the date string has it omitted. Unsure if this is a bug or feature, but hopefully this will save someone some time.<br><span class="default">&lt;?php<br></span><span class="comment">///Example<br></span><span class="default">$input </span><span class="keyword">= </span><span class="string">&quot;Feb 2010&quot;</span><span class="keyword">;<br></span><span class="default">$info </span><span class="keyword">= </span><span class="default">date_parse</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$info</span><span class="keyword">);<br><br></span><span class="comment">/*Returns:<br>array(12) { <br>&#xA0; &#xA0; [&quot;year&quot;]=&gt; int(2010)<br>&#xA0; &#xA0; [&quot;month&quot;]=&gt; int(2)<br>&#xA0; &#xA0; [&quot;day&quot;]=&gt; int(1)&#xA0; &#xA0; //&lt;---expected false like below<br>&#xA0; &#xA0; [&quot;hour&quot;]=&gt; bool(false)<br>&#xA0; &#xA0; [&quot;minute&quot;]=&gt; bool(false)<br>&#xA0; &#xA0; [&quot;second&quot;]=&gt; bool(false)<br>&#xA0; &#xA0; [&quot;fraction&quot;]=&gt; bool(false)<br>&#xA0; &#xA0; [&quot;warning_count&quot;]=&gt; int(0)<br>&#xA0; &#xA0; [&quot;warnings&quot;]=&gt; array(0) { }<br>&#xA0; &#xA0; [&quot;error_count&quot;]=&gt; int(0)<br>&#xA0; &#xA0; [&quot;errors&quot;]=&gt; array(0) { }<br>&#xA0; &#xA0; [&quot;is_localtime&quot;]=&gt; bool(false)<br>}*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse.php)

**[To root](/README.md)**