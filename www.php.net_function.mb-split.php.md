# mb_split




<div class="phpcode"><span class="html">
a (simpler) way to extract all characters from a UTF-8 string to array with a single call to a built-in function:<br><br><span class="default">&lt;?php<br>&#xA0; $str </span><span class="keyword">= </span><span class="string">&apos;&#x41C;&#x430;-<br>&#x440;&#x443;&#x441;&#x44F;&apos;</span><span class="keyword">;<br>&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;//u&apos;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Output:<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; &#x41C;<br>&#xA0; &#xA0; [1] =&gt; &#x430;<br>&#xA0; &#xA0; [2] =&gt; -<br>&#xA0; &#xA0; [3] =&gt; <br><br>&#xA0; &#xA0; [4] =&gt; &#x440;<br>&#xA0; &#xA0; [5] =&gt; &#x443;<br>&#xA0; &#xA0; [6] =&gt; &#x441;<br>&#xA0; &#xA0; [7] =&gt; &#x44F;<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
The $pattern argument doesn&apos;t use /pattern/ delimiters, unlike other regex functions such as preg_match.<br><br><span class="default">&lt;?php<br>&#xA0;&#xA0; </span><span class="comment"># Works. No slashes around the /pattern/<br>&#xA0;&#xA0; </span><span class="default">print_r</span><span class="keyword">( </span><span class="default">mb_split</span><span class="keyword">(</span><span class="string">&quot;\s&quot;</span><span class="keyword">, </span><span class="string">&quot;hello world&quot;</span><span class="keyword">) );<br>&#xA0;&#xA0; Array (<br>&#xA0; &#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="default">hello<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] =&gt; </span><span class="default">world<br>&#xA0;&#xA0; </span><span class="keyword">)<br><br>&#xA0;&#xA0; </span><span class="comment"># Doesn&apos;t work:<br>&#xA0;&#xA0; </span><span class="default">print_r</span><span class="keyword">( </span><span class="default">mb_split</span><span class="keyword">(</span><span class="string">&quot;/\s/&quot;</span><span class="keyword">, </span><span class="string">&quot;hello world&quot;</span><span class="keyword">) );<br>&#xA0;&#xA0; Array (<br>&#xA0; &#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="default">hello world<br>&#xA0;&#xA0; </span><span class="keyword">)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I figure most people will want a simple way to break-up a multibyte string into its individual characters. Here&apos;s a function I&apos;m using to do that. Change UTF-8 to your chosen encoding method.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mbStringToArray </span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$strlen </span><span class="keyword">= </span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);
<br>&#xA0; &#xA0; while (</span><span class="default">$strlen</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">,</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">,</span><span class="default">$strlen</span><span class="keyword">,</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$strlen </span><span class="keyword">= </span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$array</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In addition to Sezer Yalcin&apos;s tip.
<br>
<br>This function splits a multibyte string into an array of characters. Comparable to str_split().
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mb_str_split</span><span class="keyword">( </span><span class="default">$string </span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment"># Split at all position not after the start: ^
<br>&#xA0; &#xA0; # and not before the end: $
<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;/(?&lt;!^)(?!$)/u&apos;</span><span class="keyword">, </span><span class="default">$string </span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">$string&#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&apos;&#x706B;&#x8F66;&#x7968;&apos;</span><span class="keyword">;
<br></span><span class="default">$charlist </span><span class="keyword">= </span><span class="default">mb_str_split</span><span class="keyword">( </span><span class="default">$string </span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$charlist </span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br># Prints:
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; &#x706B;
<br>&#xA0; &#xA0; [1] =&gt; &#x8F66;
<br>&#xA0; &#xA0; [2] =&gt; &#x7968;
<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-split.php)

**[To root](/README.md)**