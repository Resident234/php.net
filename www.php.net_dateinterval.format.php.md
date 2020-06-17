# DateInterval::format




<div class="phpcode"><span class="html">
How to easy recalculate carry over points:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">DateIntervalEnhanced </span><span class="keyword">extends </span><span class="default">DateInterval </span><span class="keyword">{<br><br>&#xA0; &#xA0; public function </span><span class="default">recalculate</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$from </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$to </span><span class="keyword">= clone </span><span class="default">$from</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$to </span><span class="keyword">= </span><span class="default">$to</span><span class="keyword">-&gt;</span><span class="default">add</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$diff </span><span class="keyword">= </span><span class="default">$from</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$to</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$diff </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$k </span><span class="keyword">= </span><span class="default">$v</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">$di </span><span class="keyword">= new </span><span class="default">DateIntervalEnhanced</span><span class="keyword">(</span><span class="string">&apos;PT3600S&apos;</span><span class="keyword">);<br>echo </span><span class="string">&quot;Instead of &quot; </span><span class="keyword">. </span><span class="default">$di</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;%h:%i:%s&apos;</span><span class="keyword">) . </span><span class="string">&quot; it outputs &quot; </span><span class="keyword">. </span><span class="default">$di</span><span class="keyword">-&gt;</span><span class="default">recalculate</span><span class="keyword">()-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;%h:%i:%s&apos;</span><span class="keyword">);<br></span><span class="comment"># output will be: &quot;Instead of 0:0:3600 it outputs 1:0:0&quot;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
With php 5.3, DateTime is sweet !
<br>Here is one quick example :
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * A sweet interval formatting, will use the two biggest interval parts.
<br> * On small intervals, you get minutes and seconds.
<br> * On big intervals, you get months and days.
<br> * Only the two biggest parts are used. 
<br> * 
<br> * @param DateTime $start
<br> * @param DateTime|null $end
<br> * @return string
<br> */
<br></span><span class="keyword">public function </span><span class="default">formatDateDiff</span><span class="keyword">(</span><span class="default">$start</span><span class="keyword">, </span><span class="default">$end</span><span class="keyword">=</span><span class="default">null</span><span class="keyword">) {
<br>&#xA0; &#xA0; if(!(</span><span class="default">$start </span><span class="keyword">instanceof </span><span class="default">DateTime</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$start </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$start</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if(</span><span class="default">$end </span><span class="keyword">=== </span><span class="default">null</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$end </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if(!(</span><span class="default">$end </span><span class="keyword">instanceof </span><span class="default">DateTime</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$end </span><span class="keyword">= new </span><span class="default">DateTime</span><span class="keyword">(</span><span class="default">$start</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$interval </span><span class="keyword">= </span><span class="default">$end</span><span class="keyword">-&gt;</span><span class="default">diff</span><span class="keyword">(</span><span class="default">$start</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$doPlural </span><span class="keyword">= function(</span><span class="default">$nb</span><span class="keyword">,</span><span class="default">$str</span><span class="keyword">){return </span><span class="default">$nb</span><span class="keyword">&gt;</span><span class="default">1</span><span class="keyword">?</span><span class="default">$str</span><span class="keyword">.</span><span class="string">&apos;s&apos;</span><span class="keyword">:</span><span class="default">$str</span><span class="keyword">;}; </span><span class="comment">// adds plurals
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$format </span><span class="keyword">= array();
<br>&#xA0; &#xA0; if(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">y </span><span class="keyword">!== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format</span><span class="keyword">[] = </span><span class="string">&quot;%y &quot;</span><span class="keyword">.</span><span class="default">$doPlural</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">y</span><span class="keyword">, </span><span class="string">&quot;year&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">m </span><span class="keyword">!== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format</span><span class="keyword">[] = </span><span class="string">&quot;%m &quot;</span><span class="keyword">.</span><span class="default">$doPlural</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">m</span><span class="keyword">, </span><span class="string">&quot;month&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">d </span><span class="keyword">!== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format</span><span class="keyword">[] = </span><span class="string">&quot;%d &quot;</span><span class="keyword">.</span><span class="default">$doPlural</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">d</span><span class="keyword">, </span><span class="string">&quot;day&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">h </span><span class="keyword">!== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format</span><span class="keyword">[] = </span><span class="string">&quot;%h &quot;</span><span class="keyword">.</span><span class="default">$doPlural</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">h</span><span class="keyword">, </span><span class="string">&quot;hour&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">i </span><span class="keyword">!== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format</span><span class="keyword">[] = </span><span class="string">&quot;%i &quot;</span><span class="keyword">.</span><span class="default">$doPlural</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">i</span><span class="keyword">, </span><span class="string">&quot;minute&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">s </span><span class="keyword">!== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">count</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&quot;less than a minute ago&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format</span><span class="keyword">[] = </span><span class="string">&quot;%s &quot;</span><span class="keyword">.</span><span class="default">$doPlural</span><span class="keyword">(</span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">s</span><span class="keyword">, </span><span class="string">&quot;second&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">// We use the two biggest parts
<br>&#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">count</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">) &gt; </span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format </span><span class="keyword">= </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">).</span><span class="string">&quot; and &quot;</span><span class="keyword">.</span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">);
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format </span><span class="keyword">= </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">// Prepend &apos;since &apos; or whatever you like
<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$interval</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/dateinterval.format.php)

**[To root](/README.md)**