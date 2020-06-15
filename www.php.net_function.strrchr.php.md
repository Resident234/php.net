# strrchr




<div class="phpcode"><span class="html">
To extract your portion of a string without the actual character you searched for, you can use:<br><br><span class="default">&lt;?php<br>$path </span><span class="keyword">= </span><span class="string">&apos;/www/public_html/index.html&apos;</span><span class="keyword">;<br></span><span class="default">$filename </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">strrchr</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="string">&quot;/&quot;</span><span class="keyword">), </span><span class="default">1</span><span class="keyword">);<br>echo </span><span class="default">$filename</span><span class="keyword">; </span><span class="comment">// &quot;index.html&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * Removes the preceeding or proceeding portion of a string
<br> * relative to the last occurrence of the specified character.
<br> * The character selected may be retained or discarded.
<br> * 
<br> * Example usage:
<br> * &lt;code&gt;
<br> * $example = &apos;<a href="http://example.com/path/file.php" rel="nofollow" target="_blank">http://example.com/path/file.php</a>&apos;;
<br> * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;left&apos;, true);
<br> * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;left&apos;, false);
<br> * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;right&apos;, true);
<br> * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;right&apos;, false);
<br> * foreach($cwd_relative as $string) {
<br> *&#xA0; &#xA0;&#xA0; echo &quot;$string &lt;br&gt;&quot;.PHP_EOL;
<br> * }
<br> * &lt;/code&gt;
<br> * 
<br> * Outputs:
<br> * &lt;code&gt;
<br> * <a href="http://example.com/path/" rel="nofollow" target="_blank">http://example.com/path/</a>
<br> * <a href="http://example.com/path" rel="nofollow" target="_blank">http://example.com/path</a>
<br> * /file.php
<br> * file.php
<br> * &lt;/code&gt;
<br> * 
<br> * @param string $character the character to search for.
<br> * @param string $string the string to search through.
<br> * @param string $side determines whether text to the left or the right of the character is returned.
<br> * Options are: left, or right.
<br> * @param bool $keep_character determines whether or not to keep the character.
<br> * Options are: true, or false.
<br> * @return string
<br> */
<br></span><span class="keyword">function </span><span class="default">cut_string_using_last</span><span class="keyword">(</span><span class="default">$character</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">, </span><span class="default">$side</span><span class="keyword">, </span><span class="default">$keep_character</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$offset </span><span class="keyword">= (</span><span class="default">$keep_character </span><span class="keyword">? </span><span class="default">1 </span><span class="keyword">: </span><span class="default">0</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$whole_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$right_length </span><span class="keyword">= (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">strrchr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$character</span><span class="keyword">)) - </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$left_length </span><span class="keyword">= (</span><span class="default">$whole_length </span><span class="keyword">- </span><span class="default">$right_length </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; switch(</span><span class="default">$side</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;left&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$piece </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, (</span><span class="default">$left_length </span><span class="keyword">+ </span><span class="default">$offset</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;right&apos;</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$start </span><span class="keyword">= (</span><span class="default">0 </span><span class="keyword">- (</span><span class="default">$right_length </span><span class="keyword">+ </span><span class="default">$offset</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$piece </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$start</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; default:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$piece </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return(</span><span class="default">$piece</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strrchr.php)

**[To root](/README.md)**