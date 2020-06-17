# wordwrap




<div class="phpcode"><span class="html">
Another solution to utf-8 safe wordwrap, unsing regular expressions.<br>Pretty good performance and works in linear time.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">utf8_wordwrap</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$width</span><span class="keyword">=</span><span class="default">75</span><span class="keyword">, </span><span class="default">$break</span><span class="keyword">=</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$cut</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">)<br>{<br>&#xA0; if(</span><span class="default">$cut</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// Match anything 1 to $width chars long followed by whitespace or EOS,<br>&#xA0; &#xA0; // otherwise match anything $width chars long<br>&#xA0; &#xA0; </span><span class="default">$search </span><span class="keyword">= </span><span class="string">&apos;/(.{1,&apos;</span><span class="keyword">.</span><span class="default">$width</span><span class="keyword">.</span><span class="string">&apos;})(?:\s|$)|(.{&apos;</span><span class="keyword">.</span><span class="default">$width</span><span class="keyword">.</span><span class="string">&apos;})/uS&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$replace </span><span class="keyword">= </span><span class="string">&apos;$1$2&apos;</span><span class="keyword">.</span><span class="default">$break</span><span class="keyword">;<br>&#xA0; } else {<br>&#xA0; &#xA0; </span><span class="comment">// Anchor the beginning of the pattern with a lookahead<br>&#xA0; &#xA0; // to avoid crazy backtracking when words are longer than $width<br>&#xA0; &#xA0; </span><span class="default">$pattern </span><span class="keyword">= </span><span class="string">&apos;/(?=\s)(.{1,&apos;</span><span class="keyword">.</span><span class="default">$width</span><span class="keyword">.</span><span class="string">&apos;})(?:\s|$)/uS&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$replace </span><span class="keyword">= </span><span class="string">&apos;$1&apos;</span><span class="keyword">.</span><span class="default">$break</span><span class="keyword">;<br>&#xA0; }<br>&#xA0; return </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="default">$search</span><span class="keyword">, </span><span class="default">$replace</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span>Of course don&apos;t forget to use preg_quote on the $width and $break parameters if they come from untrusted input.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those interested in wrapping text to fit a width in *pixels* (instead of characters), you might find the following function useful; particularly for line-wrapping text over dynamically-generated images.<br><br>If a word is too long to squeeze into the available space, it&apos;ll hyphenate it as needed so it fits the container. This operates recursively, so ridiculously long words or names (e.g., URLs or this guy&apos;s signature - <a href="http://en.wikipedia.org/wiki/Wolfe+585,_Senior" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Wolfe+585,_Senior</a>) will still keep getting broken off after they&apos;ve passed the fourth or fifth lines, or whatever.<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Wraps a string to a given number of pixels.<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * This function operates in a similar fashion as PHP&apos;s native wordwrap function; however,<br>&#xA0; &#xA0;&#xA0; * it calculates wrapping based on font and point-size, rather than character count. This<br>&#xA0; &#xA0;&#xA0; * can generate more even wrapping for sentences with a consider number of thin characters.<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * @static $mult;<br>&#xA0; &#xA0;&#xA0; * @param string $text - Input string.<br>&#xA0; &#xA0;&#xA0; * @param float $width - Width, in pixels, of the text&apos;s wrapping area.<br>&#xA0; &#xA0;&#xA0; * @param float $size - Size of the font, expressed in pixels.<br>&#xA0; &#xA0;&#xA0; * @param string $font - Path to the typeface to measure the text with.<br>&#xA0; &#xA0;&#xA0; * @return string The original string with line-breaks manually inserted at detected wrapping points.<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">pixel_word_wrap</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">, </span><span class="default">$width</span><span class="keyword">, </span><span class="default">$size</span><span class="keyword">, </span><span class="default">$font</span><span class="keyword">){<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Passed a blank value? Bail early.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(!</span><span class="default">$text</span><span class="keyword">) return </span><span class="default">$text</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Check if imagettfbbox is expecting font-size to be declared in points or pixels.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">static </span><span class="default">$mult</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$mult&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">$mult </span><span class="keyword">?: </span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">GD_VERSION</span><span class="keyword">, </span><span class="string">&apos;2.0&apos;</span><span class="keyword">, </span><span class="string">&apos;&gt;=&apos;</span><span class="keyword">) ? </span><span class="default">.75 </span><span class="keyword">: </span><span class="default">1</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Text already fits the designated space without wrapping.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$box&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">imagettfbbox</span><span class="keyword">(</span><span class="default">$size </span><span class="keyword">* </span><span class="default">$mult</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$font</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] - </span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] / </span><span class="default">$mult </span><span class="keyword">&lt; </span><span class="default">$width</span><span class="keyword">)&#xA0; &#xA0; return </span><span class="default">$text</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Start measuring each line of our input and inject line-breaks when overflow&apos;s detected.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$length&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">0</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$words&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">preg_split</span><span class="keyword">(</span><span class="string">&apos;/\b(?=\S)|(?=\s)/&apos;</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$word_count&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; for(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$word_count</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">){<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Newline<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">PHP_EOL </span><span class="keyword">=== </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">])<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$length&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">0</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Strip any leading tabs.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(!</span><span class="default">$length</span><span class="keyword">) </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]&#xA0; &#xA0; =&#xA0; &#xA0; </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/^\t+/&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$box&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">imagettfbbox</span><span class="keyword">(</span><span class="default">$size </span><span class="keyword">* </span><span class="default">$mult</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$font</span><span class="keyword">, </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$m&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] - </span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] / </span><span class="default">$mult</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; This is one honkin&apos; long word, so try to hyphenate it.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if((</span><span class="default">$diff </span><span class="keyword">= </span><span class="default">$width </span><span class="keyword">- </span><span class="default">$m</span><span class="keyword">) &lt;= </span><span class="default">0</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$diff&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">abs</span><span class="keyword">(</span><span class="default">$diff</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Figure out which end of the word to start measuring from. Saves a few extra cycles in an already heavy-duty function.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$diff </span><span class="keyword">- </span><span class="default">$width </span><span class="keyword">&lt;= </span><span class="default">0</span><span class="keyword">)&#xA0; &#xA0; for(</span><span class="default">$s </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]); </span><span class="default">$s</span><span class="keyword">; --</span><span class="default">$s</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$box&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">imagettfbbox</span><span class="keyword">(</span><span class="default">$size </span><span class="keyword">* </span><span class="default">$mult</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$font</span><span class="keyword">, </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="default">0</span><span class="keyword">, </span><span class="default">$s</span><span class="keyword">) . </span><span class="string">&apos;-&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$width </span><span class="keyword">&gt; (</span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] - </span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] / </span><span class="default">$mult</span><span class="keyword">) + </span><span class="default">$size</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$breakpoint&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">$s</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$word_length&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for(</span><span class="default">$s </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$s </span><span class="keyword">&lt; </span><span class="default">$word_length</span><span class="keyword">; ++</span><span class="default">$s</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$box&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">imagettfbbox</span><span class="keyword">(</span><span class="default">$size </span><span class="keyword">* </span><span class="default">$mult</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$font</span><span class="keyword">, </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="default">0</span><span class="keyword">, </span><span class="default">$s</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">) . </span><span class="string">&apos;-&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$width </span><span class="keyword">&lt; (</span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] - </span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] / </span><span class="default">$mult</span><span class="keyword">) + </span><span class="default">$size</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$breakpoint&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">$s</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$breakpoint</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$w_l&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="default">0</span><span class="keyword">, </span><span class="default">$s</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">) . </span><span class="string">&apos;-&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$w_r&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">],&#xA0; &#xA0;&#xA0; </span><span class="default">$s</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]&#xA0; &#xA0; =&#xA0; &#xA0; </span><span class="default">$w_l</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$w_r</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ++</span><span class="default">$word_count</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$box&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">imagettfbbox</span><span class="keyword">(</span><span class="default">$size </span><span class="keyword">* </span><span class="default">$mult</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$font</span><span class="keyword">, </span><span class="default">$w_l</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$m&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] - </span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] / </span><span class="default">$mult</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; If there&apos;s no more room on the current line to fit the next word, start a new line.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$length </span><span class="keyword">&gt; </span><span class="default">0 </span><span class="keyword">&amp;&amp; </span><span class="default">$length </span><span class="keyword">+ </span><span class="default">$m </span><span class="keyword">&gt;= </span><span class="default">$width</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output&#xA0; &#xA0; </span><span class="keyword">.=&#xA0; &#xA0; </span><span class="default">PHP_EOL</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$length&#xA0; &#xA0; </span><span class="keyword">=&#xA0; &#xA0; </span><span class="default">0</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; If the current word is just a space, don&apos;t bother. Skip (saves a weird-looking gap in the text).<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(</span><span class="string">&apos; &apos; </span><span class="keyword">=== </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]) continue;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Write another word and increase the total length of the current line.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output&#xA0; &#xA0; </span><span class="keyword">.=&#xA0; &#xA0; </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$length </span><span class="keyword">+=&#xA0; &#xA0; </span><span class="default">$m</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$output</span><span class="keyword">;<br>&#xA0; &#xA0; };<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;d like to break long strings of text but avoid breaking html you may find this useful. It seems to be working for me, hope it works for you. Enjoy. :)
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">textWrap</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$new_text </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$text_1 </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;&gt;&apos;</span><span class="keyword">,</span><span class="default">$text</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$sizeof </span><span class="keyword">= </span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$text_1</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">$sizeof</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$text_2 </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;&lt;&apos;</span><span class="keyword">,</span><span class="default">$text_1</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!empty(</span><span class="default">$text_2</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$new_text </span><span class="keyword">.= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;#([^\n\r .]{25})#i&apos;</span><span class="keyword">, </span><span class="string">&apos;\\1&#xA0; &apos;</span><span class="keyword">, </span><span class="default">$text_2</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!empty(</span><span class="default">$text_2</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$new_text </span><span class="keyword">.= </span><span class="string">&apos;&lt;&apos; </span><span class="keyword">. </span><span class="default">$text_2</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] . </span><span class="string">&apos;&gt;&apos;</span><span class="keyword">;&#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$new_text</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.wordwrap.php)

**[To root](/README.md)**