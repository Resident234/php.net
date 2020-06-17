# date_diff




<div class="phpcode"><span class="html">
Powerful Function to get two date difference.<br><br>//////////////////////////////////////////////////////////////////////<br>//PARA: Date Should In YYYY-MM-DD Format<br>//RESULT FORMAT:<br>// &apos;%y Year %m Month %d Day %h Hours %i Minute %s Seconds&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 1 Year 3 Month 14 Day 11 Hours 49 Minute 36 Seconds<br>// &apos;%y Year %m Month %d Day&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 1 Year 3 Month 14 Days<br>// &apos;%m Month %d Day&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 3 Month 14 Day<br>// &apos;%d Day %h Hours&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 14 Day 11 Hours<br>// &apos;%d Day&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 14 Days<br>// &apos;%h Hours %i Minute %s Seconds&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 11 Hours 49 Minute 36 Seconds<br>// &apos;%i Minute %s Seconds&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 49 Minute 36 Seconds<br>// &apos;%h Hours&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 11 Hours<br>// &apos;%a Days&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 468 Days<br>//////////////////////////////////////////////////////////////////////<br>function dateDifference($date_1 , $date_2 , $differenceFormat = &apos;%a&apos; )<br>{<br>&#xA0; &#xA0; $datetime1 = date_create($date_1);<br>&#xA0; &#xA0; $datetime2 = date_create($date_2);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; $interval = date_diff($datetime1, $datetime2);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return $interval-&gt;format($differenceFormat);<br>&#xA0; &#xA0; <br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php <br></span><span class="comment">/* <br> * A mathematical decimal difference between two informed dates <br> *<br> * Author: Sergio Abreu<br> * Website: <a href="http://sites.sitesbr.net" rel="nofollow" target="_blank">http://sites.sitesbr.net</a><br> *<br> * Features: <br> * Automatic conversion on dates informed as string.<br> * Possibility of absolute values (always +) or relative (-/+)<br>*/<br><br></span><span class="keyword">function </span><span class="default">s_datediff</span><span class="keyword">( </span><span class="default">$str_interval</span><span class="keyword">, </span><span class="default">$dt_menor</span><span class="keyword">, </span><span class="default">$dt_maior</span><span class="keyword">, </span><span class="default">$relative</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">){<br><br>&#xA0; &#xA0; &#xA0;&#xA0; if( </span><span class="default">is_string</span><span class="keyword">( </span><span class="default">$dt_menor</span><span class="keyword">)) </span><span class="default">$dt_menor </span><span class="keyword">= </span><span class="default">date_create</span><span class="keyword">( </span><span class="default">$dt_menor</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0;&#xA0; if( </span><span class="default">is_string</span><span class="keyword">( </span><span class="default">$dt_maior</span><span class="keyword">)) </span><span class="default">$dt_maior </span><span class="keyword">= </span><span class="default">date_create</span><span class="keyword">( </span><span class="default">$dt_maior</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$diff </span><span class="keyword">= </span><span class="default">date_diff</span><span class="keyword">( </span><span class="default">$dt_menor</span><span class="keyword">, </span><span class="default">$dt_maior</span><span class="keyword">, ! </span><span class="default">$relative</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0;&#xA0; switch( </span><span class="default">$str_interval</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case </span><span class="string">&quot;y&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$total </span><span class="keyword">= </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">/ </span><span class="default">12 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">d </span><span class="keyword">/ </span><span class="default">365.25</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case </span><span class="string">&quot;m&quot;</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$total</span><span class="keyword">= </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">* </span><span class="default">12 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">d</span><span class="keyword">/</span><span class="default">30 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">h </span><span class="keyword">/ </span><span class="default">24</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case </span><span class="string">&quot;d&quot;</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$total </span><span class="keyword">= </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">* </span><span class="default">365.25 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">* </span><span class="default">30 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">d </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">h</span><span class="keyword">/</span><span class="default">24 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">i </span><span class="keyword">/ </span><span class="default">60</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case </span><span class="string">&quot;h&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$total </span><span class="keyword">= (</span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">* </span><span class="default">365.25 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">* </span><span class="default">30 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">d</span><span class="keyword">) * </span><span class="default">24 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">h </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">i</span><span class="keyword">/</span><span class="default">60</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case </span><span class="string">&quot;i&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$total </span><span class="keyword">= ((</span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">* </span><span class="default">365.25 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">* </span><span class="default">30 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">d</span><span class="keyword">) * </span><span class="default">24 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">h</span><span class="keyword">) * </span><span class="default">60 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">i </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">s</span><span class="keyword">/</span><span class="default">60</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case </span><span class="string">&quot;s&quot;</span><span class="keyword">: <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$total </span><span class="keyword">= (((</span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">* </span><span class="default">365.25 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">* </span><span class="default">30 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">d</span><span class="keyword">) * </span><span class="default">24 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">h</span><span class="keyword">) * </span><span class="default">60 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">i</span><span class="keyword">)*</span><span class="default">60 </span><span class="keyword">+ </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">s</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0;&#xA0; if( </span><span class="default">$diff</span><span class="keyword">-&gt;</span><span class="default">invert</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return -</span><span class="default">1 </span><span class="keyword">* </span><span class="default">$total</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0;&#xA0; else&#xA0; &#xA0; return </span><span class="default">$total</span><span class="keyword">;<br>&#xA0;&#xA0; }<br><br></span><span class="comment">/* Enjoy and feedback me ;-) */<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.date-diff.php)

**[To root](/README.md)**