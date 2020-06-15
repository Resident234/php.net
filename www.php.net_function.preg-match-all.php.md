# preg_match_all




<div class="phpcode"><span class="html">
if you want to extract all {token}s from a string:<br><br><span class="default">&lt;?php<br>$pattern </span><span class="keyword">= </span><span class="string">&quot;/{[^}]*}/&quot;</span><span class="keyword">;<br></span><span class="default">$subject </span><span class="keyword">= </span><span class="string">&quot;{token1} foo {token2} bar&quot;</span><span class="keyword">;<br></span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$subject</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>output:<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; {token1}<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; {token2}<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
The code that john at mccarthy dot net posted is not necessary. If you want your results grouped by individual match simply use:<br><br>&lt;?<br>preg_match_all($pattern, $string, $matches, PREG_SET_ORDER);<br>?&gt;<br><br>E.g.<br><br>&lt;?<br>preg_match_all(&apos;/([GH])([12])([!?])/&apos;, &apos;G1? H2!&apos;, $matches); // Default PREG_PATTERN_ORDER<br>// $matches = array(0 =&gt; array(0 =&gt; &apos;G1?&apos;, 1 =&gt; &apos;H2!&apos;),<br>//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1 =&gt; array(0 =&gt; &apos;G&apos;, 1 =&gt; &apos;H&apos;),<br>//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 2 =&gt; array(0 =&gt; &apos;1&apos;, 1 =&gt; &apos;2&apos;),<br>//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 3 =&gt; array(0 =&gt; &apos;?&apos;, 1 =&gt; &apos;!&apos;))<br><br>preg_match_all(&apos;/([GH])([12])([!?])/&apos;, &apos;G1? H2!&apos;, $matches, PREG_SET_ORDER);<br>// $matches = array(0 =&gt; array(0 =&gt; &apos;G1?&apos;, 1 =&gt; &apos;G&apos;, 2 =&gt; &apos;1&apos;, 3 =&gt; &apos;?&apos;),<br>//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1 =&gt; array(0 =&gt; &apos;H2!&apos;, 1 =&gt; &apos;H&apos;, 2 =&gt; &apos;2&apos;, 3 =&gt; &apos;!&apos;))<br>?&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-match-all.php)

**[To root](/README.md)**