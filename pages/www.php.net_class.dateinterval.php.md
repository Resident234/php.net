# The DateInterval class




<div class="phpcode"><span class="html">
IRT note below: from PHP 7.0+, split seconds get returned, too, and will be under the `f` property.</span>
</div>
  

#


<div class="phpcode"><span class="html">
DateInterval does not support split seconds (microseconds or milliseconds etc.) when doing a diff between two DateTime objects that contain microseconds.
<br>
<br>So you cannot do the following, for example:
<br>
<br><span class="default">&lt;?php
<br>$d1</span><span class="keyword">=new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;2012-07-08 11:14:15.638276&quot;</span><span class="keyword">);
<br></span><span class="default">$d2</span><span class="keyword">=new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;2012-07-08 11:14:15.889342&quot;</span><span class="keyword">);
<br></span><span class="default">$diff</span><span class="keyword">=</span><span class="default">$d2</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$d1</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$diff </span><span class="keyword">) ;
<br>
<br></span><span class="comment">/* returns:
<br>
<br>DateInterval Object
<br>(
<br>&#xA0; &#xA0; [y] =&gt; 0
<br>&#xA0; &#xA0; [m] =&gt; 0
<br>&#xA0; &#xA0; [d] =&gt; 0
<br>&#xA0; &#xA0; [h] =&gt; 0
<br>&#xA0; &#xA0; [i] =&gt; 0
<br>&#xA0; &#xA0; [s] =&gt; 0
<br>&#xA0; &#xA0; [invert] =&gt; 0
<br>&#xA0; &#xA0; [days] =&gt; 0
<br>)
<br>
<br>*/
<br></span><span class="default">?&gt;
<br></span>
<br>You get back 0 when you actually want to get 0.251066 seconds.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just tried the code submitted earlier on php 7.2.16:<br><br><span class="default">&lt;?php<br>$d1</span><span class="keyword">=new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;2012-07-08 11:14:15.638276&quot;</span><span class="keyword">);<br></span><span class="default">$d2</span><span class="keyword">=new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="string">&quot;2012-07-08 11:14:15.889342&quot;</span><span class="keyword">);<br></span><span class="default">$diff</span><span class="keyword">=</span><span class="default">$d2</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$d1</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$diff </span><span class="keyword">) ;<br></span><span class="default">?&gt;<br></span><br>and it now DOES return microsecond differences, in the key &apos;f&apos;:<br><br><span class="default">&lt;?php<br>DateInterval Object<br></span><span class="keyword">(<br>&#xA0; &#xA0; [</span><span class="default">y</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">m</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">d</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">h</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">i</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">s</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">f</span><span class="keyword">] =&gt; </span><span class="default">0.251066<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">weekday</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">weekday_behavior</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">first_last_day_of</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">invert</span><span class="keyword">] =&gt; </span><span class="default">1<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">days</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">special_type</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">special_amount</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">have_weekday_relative</span><span class="keyword">] =&gt; </span><span class="default">0<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">have_special_relative</span><span class="keyword">] =&gt; </span><span class="default">0<br></span><span class="keyword">)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to convert a Timespan given in Seconds into an DateInterval Object you could dot the following:<br><br><span class="default">&lt;?php<br><br>$dv </span><span class="keyword">= new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&apos;PT&apos;</span><span class="keyword">.</span><span class="default">$timespan</span><span class="keyword">.</span><span class="string">&apos;S&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>but wenn you look at the object, only the $dv-&gt;s property is set. <br><br>As stated in the documentation to DateInterval::format<br><br>The DateInterval::format() method does not recalculate carry over points in time strings nor in date segments. This is expected because it is not possible to overflow values like &quot;32 days&quot; which could be interpreted as anything from &quot;1 month and 4 days&quot; to &quot;1 month and 1 day&quot;. <br><br>If you still want to calculate the seconds into hours / days / years, etc do the following:<br><br><span class="default">&lt;?php<br><br>$d1 </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">();<br></span><span class="default">$d2 </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">();<br></span><span class="default">$d2</span><span class="keyword">-&gt;</span><span class="default">add</span><span class="keyword">(new </span><span class="default">DateInterval</span><span class="keyword">(</span><span class="string">&apos;PT&apos;</span><span class="keyword">.</span><span class="default">$timespan</span><span class="keyword">.</span><span class="string">&apos;S&apos;</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; <br></span><span class="default">$iv </span><span class="keyword">= </span><span class="default">$d2</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$d1</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>$iv is an DateInterval set with days, years, hours, seconds, etc ...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.dateinterval.php)

**[To root](/README.md)**