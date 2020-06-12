# preg_split




<div class="phpcode"><span class="html">
Sometimes PREG_SPLIT_DELIM_CAPTURE does strange results.<br><br><span class="default">&lt;?php<br>$content </span><span class="keyword">= </span><span class="string">&apos;&lt;strong&gt;Lorem ipsum dolor&lt;/strong&gt; sit &lt;img src=&quot;test.png&quot; /&gt;amet &lt;span class=&quot;test&quot; style=&quot;color:red&quot;&gt;consec&lt;i&gt;tet&lt;/i&gt;uer&lt;/span&gt;.&apos;</span><span class="keyword">;<br></span><span class="default">$chars </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;/&lt;[^&gt;]*[^\/]&gt;/i&apos;</span><span class="keyword">, </span><span class="default">$content</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY </span><span class="keyword">| </span><span class="default">PREG_SPLIT_DELIM_CAPTURE</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$chars</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>Produces:<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Lorem ipsum dolor<br>&#xA0; &#xA0; [1] =&gt;&#xA0; sit &lt;img src=&quot;test.png&quot; /&gt;amet <br>&#xA0; &#xA0; [2] =&gt; consec<br>&#xA0; &#xA0; [3] =&gt; tet<br>&#xA0; &#xA0; [4] =&gt; uer<br>)<br><br>So that the delimiter patterns are missing. If you wanna get these patters remember to use parentheses.<br><br><span class="default">&lt;?php<br>$chars </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;/(&lt;[^&gt;]*[^\/]&gt;)/i&apos;</span><span class="keyword">, </span><span class="default">$content</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY </span><span class="keyword">| </span><span class="default">PREG_SPLIT_DELIM_CAPTURE</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$chars</span><span class="keyword">); </span><span class="comment">//parentheses added<br></span><span class="default">?&gt;<br></span>Produces:<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; &lt;strong&gt;<br>&#xA0; &#xA0; [1] =&gt; Lorem ipsum dolor<br>&#xA0; &#xA0; [2] =&gt; &lt;/strong&gt;<br>&#xA0; &#xA0; [3] =&gt;&#xA0; sit &lt;img src=&quot;test.png&quot; /&gt;amet <br>&#xA0; &#xA0; [4] =&gt; &lt;span class=&quot;test&quot; style=&quot;color:red&quot;&gt;<br>&#xA0; &#xA0; [5] =&gt; consec<br>&#xA0; &#xA0; [6] =&gt; &lt;i&gt;<br>&#xA0; &#xA0; [7] =&gt; tet<br>&#xA0; &#xA0; [8] =&gt; &lt;/i&gt;<br>&#xA0; &#xA0; [9] =&gt; uer<br>&#xA0; &#xA0; [10] =&gt; &lt;/span&gt;<br>&#xA0; &#xA0; [11] =&gt; .<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Extending m.timmermans&apos;s solution, you can use the following code as a search expression parser:<br><br><span class="default">&lt;?php<br>$search_expression </span><span class="keyword">= </span><span class="string">&quot;apple bear \&quot;Tom Cruise\&quot; or &apos;Mickey Mouse&apos; another word&quot;</span><span class="keyword">;<br></span><span class="default">$words </span><span class="keyword">= </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&quot;/[\s,]*\\\&quot;([^\\\&quot;]+)\\\&quot;[\s,]*|&quot; </span><span class="keyword">. </span><span class="string">&quot;[\s,]*&apos;([^&apos;]+)&apos;[\s,]*|&quot; </span><span class="keyword">. </span><span class="string">&quot;[\s,]+/&quot;</span><span class="keyword">, </span><span class="default">$search_expression</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">PREG_SPLIT_NO_EMPTY </span><span class="keyword">| </span><span class="default">PREG_SPLIT_DELIM_CAPTURE</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The result will be:<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; apple<br>&#xA0; &#xA0; [1] =&gt; bear<br>&#xA0; &#xA0; [2] =&gt; Tom Cruise<br>&#xA0; &#xA0; [3] =&gt; or<br>&#xA0; &#xA0; [4] =&gt; Mickey Mouse<br>&#xA0; &#xA0; [5] =&gt; another<br>&#xA0; &#xA0; [6] =&gt; word<br>)<br><br>1. Accepted delimiters: white spaces (space, tab, new line etc.) and commas.<br><br>2. You can use either simple (&apos;) or double (&quot;) quotes for expressions which contains more than one word.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to split by a char, but want to ignore that char in case it is escaped, use a lookbehind assertion.
<br>
<br>In this example a string will be split by &quot;:&quot; but &quot;\:&quot; will be ignored:
<br>
<br><span class="default">&lt;?php
<br>$string</span><span class="keyword">=</span><span class="string">&apos;a:b:c\:d&apos;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">=</span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;#(?&lt;!\\\)\:#&apos;</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Results into:
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; a
<br>&#xA0; &#xA0; [1] =&gt; b
<br>&#xA0; &#xA0; [2] =&gt; c\:d
<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is another way to split a CamelCase string, which is a simpler expression than the one using lookaheads and lookbehinds: <br><br>preg_split(&apos;/([[:upper:]][[:lower:]]+)/&apos;, $last, null, PREG_SPLIT_DELIM_CAPTURE|PREG_SPLIT_NO_EMPTY)<br><br>It makes the entire CamelCased word the delimiter, then returns the delimiters (PREG_SPLIT_DELIM_CAPTURE) and omits the empty values between the delimiters (PREG_SPLIT_NO_EMPTY)</span>
</div>
  

#


<div class="phpcode"><span class="html">
To clarify the &quot;limit&quot; parameter and the PREG_SPLIT_DELIM_CAPTURE option,
<br>
<br><span class="default">&lt;?php
<br>$preg_split</span><span class="keyword">(</span><span class="string">&apos;(/ /)&apos;</span><span class="keyword">, </span><span class="string">&apos;1 2 3 4 5 6 7 8&apos;</span><span class="keyword">, </span><span class="default">4 </span><span class="keyword">,</span><span class="default">PREG_SPLIT_DELIM_CAPTURE </span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>returns:
<br>
<br>(&apos;1&apos;, &apos; &apos;, &apos;2&apos;, &apos; &apos; , &apos;3&apos;, &apos; &apos;, &apos;4 5 6 7 8&apos;)
<br>
<br>So you actually get 7 array items not 4</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-split.php)

**[⬆ to root](/)**