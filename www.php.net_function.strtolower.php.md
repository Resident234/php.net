# strtolower




<div class="phpcode"><span class="html">
strtolower(); doesn&apos;t work for polish chars
<br>
<br><span class="default">&lt;?php strtolower</span><span class="keyword">(</span><span class="string">&quot;m&#x104;kA&quot;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>will return: m&#x104;ka;
<br>
<br>the best solution - use mb_strtolower()
<br>
<br><span class="default">&lt;?php mb_strtolower</span><span class="keyword">(</span><span class="string">&quot;m&#x104;kA&quot;</span><span class="keyword">,</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>will return: m&#x105;ka</span>
</div>
  

#


<div class="phpcode"><span class="html">
for cyrillic and UTF 8 use&#xA0; mb_convert_case
<br>
<br>exampel
<br>
<br><span class="default">&lt;?php
<br>$string </span><span class="keyword">= </span><span class="string">&quot;&#x410;&#x432;&#x441;&#x442;&#x440;&#x430;&#x43B;&#x438;&#x44F;&quot;</span><span class="keyword">;
<br></span><span class="default">$string </span><span class="keyword">= </span><span class="default">mb_convert_case</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">MB_CASE_LOWER</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);
<br>echo </span><span class="default">$string</span><span class="keyword">;
<br>
<br></span><span class="comment">//output is: &#x430;&#x432;&#x441;&#x442;&#x440;&#x430;&#x43B;&#x438;&#x44F;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is worth noting that <br><span class="default">&lt;?php <br>var_dump</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">))<br></span><span class="default">?&gt;<br></span>returns: <br>string(0) &quot;&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
the function&#xA0; arraytolower will create duplicate entries since keys are case sensitive.&#xA0; 
<br>
<br><span class="default">&lt;?php
<br>$array </span><span class="keyword">= array(</span><span class="string">&apos;test1&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;asgAFasDAAd&apos;</span><span class="keyword">, </span><span class="string">&apos;TEST2&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;ASddhshsDGb&apos;</span><span class="keyword">, </span><span class="string">&apos;TeSt3 &apos;</span><span class="keyword">=&gt; </span><span class="string">&apos;asdasda@asdadadASDASDgh&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">arraytolower</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>/*
<br>Array
<br>(
<br>&#xA0; &#xA0; [test1] =&gt; asgafasdaad
<br>&#xA0; &#xA0; [TEST2] =&gt; ASddhshsDGb
<br>&#xA0; &#xA0; [TeSt3] =&gt; asdasda@asdadadASDASDgh
<br>&#xA0; &#xA0; [test2] =&gt; asddhshsdgb
<br>&#xA0; &#xA0; [test3] =&gt; asdasda@asdadadasdasdgh
<br>)
<br>*/
<br>
<br>I prefer this method
<br>
<br><span class="default">&lt;?php
<br>&#xA0; </span><span class="keyword">function </span><span class="default">arraytolower</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$include_leys</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">) {
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if(</span><span class="default">$include_leys</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">)] = </span><span class="default">arraytolower</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$include_leys</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">)] = </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">$array2</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">arraytolower</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$include_leys</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);&#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; return </span><span class="default">$array</span><span class="keyword">;
<br>&#xA0; } 
<br></span><span class="default">?&gt;
<br></span>
<br>which when used like this
<br>
<br><span class="default">&lt;?php
<br>$array </span><span class="keyword">= </span><span class="default">$array </span><span class="keyword">= array(</span><span class="string">&apos;test1&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;asgAFasDAAd&apos;</span><span class="keyword">, </span><span class="string">&apos;TEST2&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;ASddhshsDGb&apos;</span><span class="keyword">, </span><span class="string">&apos;TeSt3 &apos;</span><span class="keyword">=&gt; </span><span class="string">&apos;asdasda@asdadadASDASDgh&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">$array1 </span><span class="keyword">= </span><span class="default">arraytolower</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);
<br></span><span class="default">$array2 </span><span class="keyword">= </span><span class="default">arraytolower</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>will give output of 
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [test1] =&gt; asgafasdaad
<br>&#xA0; &#xA0; [TEST2] =&gt; asddhshsdgb
<br>&#xA0; &#xA0; [TeSt3] =&gt; asdasda@asdadadasdasdgh
<br>)
<br>Array
<br>(
<br>&#xA0; &#xA0; [test1] =&gt; asgafasdaad
<br>&#xA0; &#xA0; [test2] =&gt; asddhshsdgb
<br>&#xA0; &#xA0; [test3] =&gt; asdasda@asdadadasdasdgh
<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strtolower.php)

**[To root](/README.md)**