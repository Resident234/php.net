# levenshtein




<div class="phpcode"><span class="html">
The levenshtein function processes each byte of the input string individually. Then for multibyte encodings, such as UTF-8, it may give misleading results.<br><br>Example with a french accented word :<br>- levenshtein(&apos;notre&apos;, &apos;votre&apos;) = 1<br>- levenshtein(&apos;notre&apos;, &apos;n&#xF4;tre&apos;) = 2 (huh ?!)<br><br>You can easily find a multibyte compliant PHP implementation of the levenshtein function but it will be of course much slower than the C implementation.<br><br>Another option is to convert the strings to a single-byte (lossless) encoding so that they can feed the fast core levenshtein function.<br><br>Here is the conversion function I used with a search engine storing UTF-8 strings, and a quick benchmark. I hope it will help.<br><br><span class="default">&lt;?php<br></span><span class="comment">// Convert an UTF-8 encoded string to a single-byte string suitable for<br>// functions such as levenshtein.<br>// <br>// The function simply uses (and updates) a tailored dynamic encoding<br>// (in/out map parameter) where non-ascii characters are remapped to<br>// the range [128-255] in order of appearance.<br>//<br>// Thus it supports up to 128 different multibyte code points max over<br>// the whole set of strings sharing this encoding.<br>//<br></span><span class="keyword">function </span><span class="default">utf8_to_extended_ascii</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, &amp;</span><span class="default">$map</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// find all multibyte characters (cf. utf-8 encoding specs)<br>&#xA0; &#xA0; </span><span class="default">$matches </span><span class="keyword">= array();<br>&#xA0; &#xA0; if (!</span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="string">&apos;/[\xC0-\xF7][\x80-\xBF]+/&apos;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$str</span><span class="keyword">; </span><span class="comment">// plain ascii string<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; // update the encoding map with the characters not already met<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] as </span><span class="default">$mbc</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!isset(</span><span class="default">$map</span><span class="keyword">[</span><span class="default">$mbc</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$map</span><span class="keyword">[</span><span class="default">$mbc</span><span class="keyword">] = </span><span class="default">chr</span><span class="keyword">(</span><span class="default">128 </span><span class="keyword">+ </span><span class="default">count</span><span class="keyword">(</span><span class="default">$map</span><span class="keyword">));<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// finally remap non-ascii characters<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">$map</span><span class="keyword">);<br>}<br><br></span><span class="comment">// Didactic example showing the usage of the previous conversion function but,<br>// for better performance, in a real application with a single input string<br>// matched against many strings from a database, you will probably want to<br>// pre-encode the input only once.<br>//<br></span><span class="keyword">function </span><span class="default">levenshtein_utf8</span><span class="keyword">(</span><span class="default">$s1</span><span class="keyword">, </span><span class="default">$s2</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$charMap </span><span class="keyword">= array();<br>&#xA0; &#xA0; </span><span class="default">$s1 </span><span class="keyword">= </span><span class="default">utf8_to_extended_ascii</span><span class="keyword">(</span><span class="default">$s1</span><span class="keyword">, </span><span class="default">$charMap</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$s2 </span><span class="keyword">= </span><span class="default">utf8_to_extended_ascii</span><span class="keyword">(</span><span class="default">$s2</span><span class="keyword">, </span><span class="default">$charMap</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="default">levenshtein</span><span class="keyword">(</span><span class="default">$s1</span><span class="keyword">, </span><span class="default">$s2</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Results (for about 6000 calls)<br>- reference time core C function (single-byte) : 30 ms<br>- utf8 to ext-ascii conversion + core function : 90 ms<br>- full php implementation : 3000 ms</span>
</div>
  

#


<div class="phpcode"><span class="html">
[EDITOR&apos;S NOTE: original post and 2 corrections combined into 1 -- mgf]
<br>
<br>Here is an implementation of the Levenshtein Distance calculation that only uses a one-dimensional array and doesn&apos;t have a limit to the string length. This implementation was inspired by maze generation algorithms that also use only one-dimensional arrays.
<br>
<br>I have tested this function with two 532-character strings and it completed in 0.6-0.8 seconds. 
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/*
<br>* This function starts out with several checks in an attempt to save time.
<br>*&#xA0;&#xA0; 1.&#xA0; The shorter string is always used as the &quot;right-hand&quot; string (as the size of the array is based on its length).&#xA0; 
<br>*&#xA0;&#xA0; 2.&#xA0; If the left string is empty, the length of the right is returned.
<br>*&#xA0;&#xA0; 3.&#xA0; If the right string is empty, the length of the left is returned.
<br>*&#xA0;&#xA0; 4.&#xA0; If the strings are equal, a zero-distance is returned.
<br>*&#xA0;&#xA0; 5.&#xA0; If the left string is contained within the right string, the difference in length is returned.
<br>*&#xA0;&#xA0; 6.&#xA0; If the right string is contained within the left string, the difference in length is returned.
<br>* If none of the above conditions were met, the Levenshtein algorithm is used.
<br>*/
<br></span><span class="keyword">function </span><span class="default">LevenshteinDistance</span><span class="keyword">(</span><span class="default">$s1</span><span class="keyword">, </span><span class="default">$s2</span><span class="keyword">)
<br>{
<br>&#xA0; </span><span class="default">$sLeft </span><span class="keyword">= (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$s1</span><span class="keyword">) &gt; </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$s2</span><span class="keyword">)) ? </span><span class="default">$s1 </span><span class="keyword">: </span><span class="default">$s2</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$sRight </span><span class="keyword">= (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$s1</span><span class="keyword">) &gt; </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$s2</span><span class="keyword">)) ? </span><span class="default">$s2 </span><span class="keyword">: </span><span class="default">$s1</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$nLeftLength </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$sLeft</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$nRightLength </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$sRight</span><span class="keyword">);
<br>&#xA0; if (</span><span class="default">$nLeftLength </span><span class="keyword">== </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; return </span><span class="default">$nRightLength</span><span class="keyword">;
<br>&#xA0; else if (</span><span class="default">$nRightLength </span><span class="keyword">== </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; return </span><span class="default">$nLeftLength</span><span class="keyword">;
<br>&#xA0; else if (</span><span class="default">$sLeft </span><span class="keyword">=== </span><span class="default">$sRight</span><span class="keyword">)
<br>&#xA0; &#xA0; return </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; else if ((</span><span class="default">$nLeftLength </span><span class="keyword">&lt; </span><span class="default">$nRightLength</span><span class="keyword">) &amp;&amp; (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$sRight</span><span class="keyword">, </span><span class="default">$sLeft</span><span class="keyword">) !== </span><span class="default">FALSE</span><span class="keyword">))
<br>&#xA0; &#xA0; return </span><span class="default">$nRightLength </span><span class="keyword">- </span><span class="default">$nLeftLength</span><span class="keyword">;
<br>&#xA0; else if ((</span><span class="default">$nRightLength </span><span class="keyword">&lt; </span><span class="default">$nLeftLength</span><span class="keyword">) &amp;&amp; (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$sLeft</span><span class="keyword">, </span><span class="default">$sRight</span><span class="keyword">) !== </span><span class="default">FALSE</span><span class="keyword">))
<br>&#xA0; &#xA0; return </span><span class="default">$nLeftLength </span><span class="keyword">- </span><span class="default">$nRightLength</span><span class="keyword">;
<br>&#xA0; else {
<br>&#xA0; &#xA0; </span><span class="default">$nsDistance </span><span class="keyword">= </span><span class="default">range</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">, </span><span class="default">$nRightLength </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; for (</span><span class="default">$nLeftPos </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$nLeftPos </span><span class="keyword">&lt;= </span><span class="default">$nLeftLength</span><span class="keyword">; ++</span><span class="default">$nLeftPos</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$cLeft </span><span class="keyword">= </span><span class="default">$sLeft</span><span class="keyword">[</span><span class="default">$nLeftPos </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$nDiagonal </span><span class="keyword">= </span><span class="default">$nLeftPos </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$nsDistance</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="default">$nLeftPos</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; for (</span><span class="default">$nRightPos </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$nRightPos </span><span class="keyword">&lt;= </span><span class="default">$nRightLength</span><span class="keyword">; ++</span><span class="default">$nRightPos</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cRight </span><span class="keyword">= </span><span class="default">$sRight</span><span class="keyword">[</span><span class="default">$nRightPos </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$nCost </span><span class="keyword">= (</span><span class="default">$cRight </span><span class="keyword">== </span><span class="default">$cLeft</span><span class="keyword">) ? </span><span class="default">0 </span><span class="keyword">: </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$nNewDiagonal </span><span class="keyword">= </span><span class="default">$nsDistance</span><span class="keyword">[</span><span class="default">$nRightPos</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$nsDistance</span><span class="keyword">[</span><span class="default">$nRightPos</span><span class="keyword">] = 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">min</span><span class="keyword">(</span><span class="default">$nsDistance</span><span class="keyword">[</span><span class="default">$nRightPos</span><span class="keyword">] + </span><span class="default">1</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$nsDistance</span><span class="keyword">[</span><span class="default">$nRightPos </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">] + </span><span class="default">1</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$nDiagonal </span><span class="keyword">+ </span><span class="default">$nCost</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$nDiagonal </span><span class="keyword">= </span><span class="default">$nNewDiagonal</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$nsDistance</span><span class="keyword">[</span><span class="default">$nRightLength</span><span class="keyword">];
<br>&#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s also useful if you want to make some sort of registration page and you want to make sure that people who register don&apos;t pick usernames that are very similar to their passwords.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.levenshtein.php)

**[To root](/README.md)**