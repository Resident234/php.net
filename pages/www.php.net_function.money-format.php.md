# money_format




<div class="phpcode"><span class="html">
For most of us in the US, we don&apos;t want to see a &quot;USD&quot; for our currency symbol, so &apos;%i&apos; doesn&apos;t cut it.&#xA0; Here&apos;s what I used that worked to get what most&#xA0; people expect to see for a number format.<br><br>$number = 123.4<br>setlocale(LC_MONETARY, &apos;en_US.UTF-8&apos;);<br>money_format(&apos;%.2n&apos;, $number);<br><br>output:<br>$123.40<br><br>That gives me a dollar sign at the beginning, and 2 digits at the end.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is a some function posted before, however various bugs were corrected.
<br>
<br>Thank you to Stuart Roe by reporting the bug on printing signals.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/*
<br>That it is an implementation of the function money_format for the
<br>platforms that do not it bear.&#xA0; 
<br>
<br>The function accepts to same string of format accepts for the
<br>original function of the PHP.&#xA0; 
<br>
<br>(Sorry. my writing in English is very bad)&#xA0; 
<br>
<br>The function is tested using PHP 5.1.4 in Windows XP
<br>and Apache WebServer.
<br>*/
<br></span><span class="keyword">function </span><span class="default">money_format</span><span class="keyword">(</span><span class="default">$format</span><span class="keyword">, </span><span class="default">$number</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$regex&#xA0; </span><span class="keyword">= </span><span class="string">&apos;/%((?:[\^!\-]|\+|\(|\=.)*)([0-9]+)?&apos;</span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;(?:#([0-9]+))?(?:\.([0-9]+))?([in%])/&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; if (</span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_MONETARY</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">) == </span><span class="string">&apos;C&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_MONETARY</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">$locale </span><span class="keyword">= </span><span class="default">localeconv</span><span class="keyword">();
<br>&#xA0; &#xA0; </span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="default">$regex</span><span class="keyword">, </span><span class="default">$format</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">, </span><span class="default">PREG_SET_ORDER</span><span class="keyword">);
<br>&#xA0; &#xA0; foreach (</span><span class="default">$matches </span><span class="keyword">as </span><span class="default">$fmatch</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">floatval</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$flags </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;fillchar&apos;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\=(.)/&apos;</span><span class="keyword">, </span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$match</span><span class="keyword">) ?
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$match</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] : </span><span class="string">&apos; &apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;nogroup&apos;&#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\^/&apos;</span><span class="keyword">, </span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) &gt; </span><span class="default">0</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;usesignal&apos; </span><span class="keyword">=&gt; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\+|\(/&apos;</span><span class="keyword">, </span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$match</span><span class="keyword">) ?
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$match</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] : </span><span class="string">&apos;+&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;nosimbol&apos;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\!/&apos;</span><span class="keyword">, </span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) &gt; </span><span class="default">0</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;isleft&apos;&#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\-/&apos;</span><span class="keyword">, </span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) &gt; </span><span class="default">0
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$width&#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]) ? (int)</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] : </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$left&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]) ? (int)</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] : </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$right&#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">]) ? (int)</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">] : </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;int_frac_digits&apos;</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$conversion </span><span class="keyword">= </span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">];
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$positive </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$value </span><span class="keyword">&lt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$positive </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value&#xA0; </span><span class="keyword">*= -</span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$letter </span><span class="keyword">= </span><span class="default">$positive </span><span class="keyword">? </span><span class="string">&apos;p&apos; </span><span class="keyword">: </span><span class="string">&apos;n&apos;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$prefix </span><span class="keyword">= </span><span class="default">$suffix </span><span class="keyword">= </span><span class="default">$cprefix </span><span class="keyword">= </span><span class="default">$csuffix </span><span class="keyword">= </span><span class="default">$signal </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$signal </span><span class="keyword">= </span><span class="default">$positive </span><span class="keyword">? </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;positive_sign&apos;</span><span class="keyword">] : </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;negative_sign&apos;</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; switch (</span><span class="default">true</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_sign_posn&quot;</span><span class="keyword">] == </span><span class="default">1 </span><span class="keyword">&amp;&amp; </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;usesignal&apos;</span><span class="keyword">] == </span><span class="string">&apos;+&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$prefix </span><span class="keyword">= </span><span class="default">$signal</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_sign_posn&quot;</span><span class="keyword">] == </span><span class="default">2 </span><span class="keyword">&amp;&amp; </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;usesignal&apos;</span><span class="keyword">] == </span><span class="string">&apos;+&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$suffix </span><span class="keyword">= </span><span class="default">$signal</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_sign_posn&quot;</span><span class="keyword">] == </span><span class="default">3 </span><span class="keyword">&amp;&amp; </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;usesignal&apos;</span><span class="keyword">] == </span><span class="string">&apos;+&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cprefix </span><span class="keyword">= </span><span class="default">$signal</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_sign_posn&quot;</span><span class="keyword">] == </span><span class="default">4 </span><span class="keyword">&amp;&amp; </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;usesignal&apos;</span><span class="keyword">] == </span><span class="string">&apos;+&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$csuffix </span><span class="keyword">= </span><span class="default">$signal</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;usesignal&apos;</span><span class="keyword">] == </span><span class="string">&apos;(&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_sign_posn&quot;</span><span class="keyword">] == </span><span class="default">0</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$prefix </span><span class="keyword">= </span><span class="string">&apos;(&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$suffix </span><span class="keyword">= </span><span class="string">&apos;)&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;nosimbol&apos;</span><span class="keyword">]) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$currency </span><span class="keyword">= </span><span class="default">$cprefix </span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (</span><span class="default">$conversion </span><span class="keyword">== </span><span class="string">&apos;i&apos; </span><span class="keyword">? </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;int_curr_symbol&apos;</span><span class="keyword">] : </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;currency_symbol&apos;</span><span class="keyword">]) .
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$csuffix</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$currency </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$space&#xA0; </span><span class="keyword">= </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_sep_by_space&quot;</span><span class="keyword">] ? </span><span class="string">&apos; &apos; </span><span class="keyword">: </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">number_format</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">, </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;mon_decimal_point&apos;</span><span class="keyword">],
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;nogroup&apos;</span><span class="keyword">] ? </span><span class="string">&apos;&apos; </span><span class="keyword">: </span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;mon_thousands_sep&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= @</span><span class="default">explode</span><span class="keyword">(</span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;mon_decimal_point&apos;</span><span class="keyword">], </span><span class="default">$value</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$n </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$prefix</span><span class="keyword">) + </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$currency</span><span class="keyword">) + </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$left </span><span class="keyword">&gt; </span><span class="default">0 </span><span class="keyword">&amp;&amp; </span><span class="default">$left </span><span class="keyword">&gt; </span><span class="default">$n</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;fillchar&apos;</span><span class="keyword">], </span><span class="default">$left </span><span class="keyword">- </span><span class="default">$n</span><span class="keyword">) . </span><span class="default">$value</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">implode</span><span class="keyword">(</span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&apos;mon_decimal_point&apos;</span><span class="keyword">], </span><span class="default">$value</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$locale</span><span class="keyword">[</span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$letter</span><span class="keyword">}</span><span class="string">_cs_precedes&quot;</span><span class="keyword">]) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">$prefix </span><span class="keyword">. </span><span class="default">$currency </span><span class="keyword">. </span><span class="default">$space </span><span class="keyword">. </span><span class="default">$value </span><span class="keyword">. </span><span class="default">$suffix</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">$prefix </span><span class="keyword">. </span><span class="default">$value </span><span class="keyword">. </span><span class="default">$space </span><span class="keyword">. </span><span class="default">$currency </span><span class="keyword">. </span><span class="default">$suffix</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$width </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$width</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;fillchar&apos;</span><span class="keyword">], </span><span class="default">$flags</span><span class="keyword">[</span><span class="string">&apos;isleft&apos;</span><span class="keyword">] ?
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">STR_PAD_RIGHT </span><span class="keyword">: </span><span class="default">STR_PAD_LEFT</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$format </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$fmatch</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$value</span><span class="keyword">, </span><span class="default">$format</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$format</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In Rafael M. Salvioni function localeconv(); returns an invalid array in my Windows XP SP3 running PHP 5.4.13 so to prevent the Warning Message: implode(): Invalid arguments passed i just add the $locale manually. For other languages just fill the array with the correct settings.<br><br>&lt;?<br><br>&#xA0; &#xA0; &#xA0;&#xA0; $locale = array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;decimal_point&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; &apos;.&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;thousands_sep&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; &apos;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;int_curr_symbol&apos;&#xA0; &#xA0; =&gt; &apos;EUR&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;currency_symbol&apos;&#xA0; &#xA0; =&gt; &apos;&#x20AC;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;mon_decimal_point&apos;&#xA0; &#xA0; =&gt; &apos;,&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;mon_thousands_sep&apos;&#xA0; &#xA0; =&gt; &apos;.&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;positive_sign&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; &apos;&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;negative_sign&apos;&#xA0; &#xA0;&#xA0; =&gt; &apos;-&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;int_frac_digits&apos;&#xA0; &#xA0; =&gt; 2,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;frac_digits&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; 2,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;p_cs_precedes&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; 0,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;p_sep_by_space&apos;&#xA0; &#xA0; =&gt; 1,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;p_sign_posn&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; 1,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;n_sign_posn&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; 1,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;grouping&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt; array(),<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;mon_grouping&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; array(0 =&gt; 3, 1 =&gt; 3)<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; );<br>?&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
If money_format doesn&apos;t seem to be working properly, make sure you are defining a valid locale.&#xA0; For example, on Debian, &apos;en_US&apos; is not a valid locale - you need &apos;en_US.UTF-8&apos; or &apos;en_US.ISO-8559-1&apos;.<br><br>This was frustrating me for a while.&#xA0; Debian has a list of valid locales at /usr/share/i18n/SUPPORTED; find yours there if it&apos;s not working properly.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.money-format.php)

**[To root](/README.md)**