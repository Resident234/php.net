# bcadd




<div class="phpcode"><span class="html">
I made this to add an unlimited size of numbers together..
<br>
<br>This could be useful for those without the BCMath extension.
<br>
<br>It allows decimals, and optional $Scale parameter.&#xA0; If $Scale isn&apos;t specified, then it&apos;ll automatically adjust to show the correct number of decimals.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">Add</span><span class="keyword">(</span><span class="default">$Num1</span><span class="keyword">,</span><span class="default">$Num2</span><span class="keyword">,</span><span class="default">$Scale</span><span class="keyword">=</span><span class="default">null</span><span class="keyword">) {
<br>&#xA0; </span><span class="comment">// check if they&apos;re valid positive numbers, extract the whole numbers and decimals
<br>&#xA0; </span><span class="keyword">if(!</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;/^\+?(\d+)(\.\d+)?$/&quot;</span><span class="keyword">,</span><span class="default">$Num1</span><span class="keyword">,</span><span class="default">$Tmp1</span><span class="keyword">)||
<br>&#xA0; &#xA0;&#xA0; !</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;/^\+?(\d+)(\.\d+)?$/&quot;</span><span class="keyword">,</span><span class="default">$Num2</span><span class="keyword">,</span><span class="default">$Tmp2</span><span class="keyword">)) return(</span><span class="string">&apos;0&apos;</span><span class="keyword">);
<br>
<br>&#xA0; </span><span class="comment">// this is where the result is stored
<br>&#xA0; </span><span class="default">$Output</span><span class="keyword">=array();
<br>
<br>&#xA0; </span><span class="comment">// remove ending zeroes from decimals and remove point
<br>&#xA0; </span><span class="default">$Dec1</span><span class="keyword">=isset(</span><span class="default">$Tmp1</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">])?</span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$Tmp1</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">],</span><span class="default">1</span><span class="keyword">),</span><span class="string">&apos;0&apos;</span><span class="keyword">):</span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$Dec2</span><span class="keyword">=isset(</span><span class="default">$Tmp2</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">])?</span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$Tmp2</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">],</span><span class="default">1</span><span class="keyword">),</span><span class="string">&apos;0&apos;</span><span class="keyword">):</span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="comment">// calculate the longest length of decimals
<br>&#xA0; </span><span class="default">$DLen</span><span class="keyword">=</span><span class="default">max</span><span class="keyword">(</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$Dec1</span><span class="keyword">),</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$Dec2</span><span class="keyword">));
<br>
<br>&#xA0; </span><span class="comment">// if $Scale is null, automatically set it to the amount of decimal places for accuracy
<br>&#xA0; </span><span class="keyword">if(</span><span class="default">$Scale</span><span class="keyword">==</span><span class="default">null</span><span class="keyword">) </span><span class="default">$Scale</span><span class="keyword">=</span><span class="default">$DLen</span><span class="keyword">;
<br>
<br>&#xA0; </span><span class="comment">// remove leading zeroes and reverse the whole numbers, then append padded decimals on the end
<br>&#xA0; </span><span class="default">$Num1</span><span class="keyword">=</span><span class="default">strrev</span><span class="keyword">(</span><span class="default">ltrim</span><span class="keyword">(</span><span class="default">$Tmp1</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">],</span><span class="string">&apos;0&apos;</span><span class="keyword">).</span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">$Dec1</span><span class="keyword">,</span><span class="default">$DLen</span><span class="keyword">,</span><span class="string">&apos;0&apos;</span><span class="keyword">));
<br>&#xA0; </span><span class="default">$Num2</span><span class="keyword">=</span><span class="default">strrev</span><span class="keyword">(</span><span class="default">ltrim</span><span class="keyword">(</span><span class="default">$Tmp2</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">],</span><span class="string">&apos;0&apos;</span><span class="keyword">).</span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">$Dec2</span><span class="keyword">,</span><span class="default">$DLen</span><span class="keyword">,</span><span class="string">&apos;0&apos;</span><span class="keyword">));
<br>
<br>&#xA0; </span><span class="comment">// calculate the longest length we need to process
<br>&#xA0; </span><span class="default">$MLen</span><span class="keyword">=</span><span class="default">max</span><span class="keyword">(</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$Num1</span><span class="keyword">),</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$Num2</span><span class="keyword">));
<br>
<br>&#xA0; </span><span class="comment">// pad the two numbers so they are of equal length (both equal to $MLen)
<br>&#xA0; </span><span class="default">$Num1</span><span class="keyword">=</span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">$Num1</span><span class="keyword">,</span><span class="default">$MLen</span><span class="keyword">,</span><span class="string">&apos;0&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$Num2</span><span class="keyword">=</span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">$Num2</span><span class="keyword">,</span><span class="default">$MLen</span><span class="keyword">,</span><span class="string">&apos;0&apos;</span><span class="keyword">);
<br>
<br>&#xA0; </span><span class="comment">// process each digit, keep the ones, carry the tens (remainders)
<br>&#xA0; </span><span class="keyword">for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">$MLen</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; </span><span class="default">$Sum</span><span class="keyword">=((int)</span><span class="default">$Num1</span><span class="keyword">{</span><span class="default">$i</span><span class="keyword">}+(int)</span><span class="default">$Num2</span><span class="keyword">{</span><span class="default">$i</span><span class="keyword">});
<br>&#xA0; &#xA0; if(isset(</span><span class="default">$Output</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">])) </span><span class="default">$Sum</span><span class="keyword">+=</span><span class="default">$Output</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];
<br>&#xA0; &#xA0; </span><span class="default">$Output</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]=</span><span class="default">$Sum</span><span class="keyword">%</span><span class="default">10</span><span class="keyword">;
<br>&#xA0; &#xA0; if(</span><span class="default">$Sum</span><span class="keyword">&gt;</span><span class="default">9</span><span class="keyword">) </span><span class="default">$Output</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">]=</span><span class="default">1</span><span class="keyword">;
<br>&#xA0; }
<br>
<br>&#xA0; </span><span class="comment">// convert the array to string and reverse it
<br>&#xA0; </span><span class="default">$Output</span><span class="keyword">=</span><span class="default">strrev</span><span class="keyword">(</span><span class="default">implode</span><span class="keyword">(</span><span class="default">$Output</span><span class="keyword">));
<br>
<br>&#xA0; </span><span class="comment">// substring the decimal digits from the result, pad if necessary (if $Scale &gt; amount of actual decimals)
<br>&#xA0; // next, since actual zero values can cause a problem with the substring values, if so, just simply give &apos;0&apos;
<br>&#xA0; // next, append the decimal value, if $Scale is defined, and return result
<br>&#xA0; </span><span class="default">$Decimal</span><span class="keyword">=</span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$Output</span><span class="keyword">,-</span><span class="default">$DLen</span><span class="keyword">,</span><span class="default">$Scale</span><span class="keyword">),</span><span class="default">$Scale</span><span class="keyword">,</span><span class="string">&apos;0&apos;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$Output</span><span class="keyword">=((</span><span class="default">$MLen</span><span class="keyword">-</span><span class="default">$DLen</span><span class="keyword">&lt;</span><span class="default">1</span><span class="keyword">)?</span><span class="string">&apos;0&apos;</span><span class="keyword">:</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$Output</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,-</span><span class="default">$DLen</span><span class="keyword">));
<br>&#xA0; </span><span class="default">$Output</span><span class="keyword">.=((</span><span class="default">$Scale</span><span class="keyword">&gt;</span><span class="default">0</span><span class="keyword">)?</span><span class="string">&quot;.</span><span class="keyword">{</span><span class="default">$Decimal</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">:</span><span class="string">&apos;&apos;</span><span class="keyword">);
<br>&#xA0; return(</span><span class="default">$Output</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">$A</span><span class="keyword">=</span><span class="string">&quot;5650175242.508133742&quot;</span><span class="keyword">;
<br></span><span class="default">$B</span><span class="keyword">=</span><span class="string">&quot;308437806.831153821478770&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;&#xA0; Add(%s,%s);\r\n// %s\r\n\r\n&quot;</span><span class="keyword">,</span><span class="default">$A</span><span class="keyword">,</span><span class="default">$B</span><span class="keyword">,&#xA0; </span><span class="default">Add</span><span class="keyword">(</span><span class="default">$A</span><span class="keyword">,</span><span class="default">$B</span><span class="keyword">));
<br></span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;BCAdd(%s,%s);\r\n// %s\r\n\r\n&quot;</span><span class="keyword">,</span><span class="default">$A</span><span class="keyword">,</span><span class="default">$B</span><span class="keyword">,</span><span class="default">BCAdd</span><span class="keyword">(</span><span class="default">$A</span><span class="keyword">,</span><span class="default">$B</span><span class="keyword">));
<br>
<br></span><span class="comment">/*
<br>&#xA0; This will produce the following..
<br>&#xA0; &#xA0; Add(5650175242.508133742,308437806.831153821478770);
<br>&#xA0; // 5958613049.33928756347877
<br>
<br>&#xA0; BCAdd(5650175242.508133742,308437806.831153821478770);
<br>&#xA0; // 5958613049
<br>*/
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>It was a fun experience making, and thought I&apos;d share it.
<br>Enjoy,
<br>Nitrogen.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.bcadd.php)

**[To root](/README.md)**