# strlen




<div class="phpcode"><span class="html">
I want to share something seriously important for newbies or beginners of PHP who plays with strings of UTF8 encoded characters or the languages like: Arabic, Persian, Pashto, Dari, Chinese (simplified), Chinese (traditional), Japanese, Vietnamese, Urdu, Macedonian, Lithuanian, and etc.<br>As the manual says: &quot;strlen() returns the number of bytes rather than the number of characters in a string.&quot;, so if you want to get the number of characters in a string of UTF8 so use mb_strlen() instead of strlen().<br><br>Example:<br><br><span class="default">&lt;?php<br></span><span class="comment">// the Arabic (Hello) string below is: 59 bytes and 32 characters<br></span><span class="default">$utf8 </span><span class="keyword">= </span><span class="string">&quot;&#x627;&#x644;&#x633;&#x644;&#x627;&#x645; &#x639;&#x644;&#x6CC;&#x6A9;&#x645; &#x648;&#x631;&#x62D;&#x645;&#x629; &#x627;&#x644;&#x644;&#x647; &#x648;&#x628;&#x631;&#x6A9;&#x627;&#x62A;&#x647;!&quot;</span><span class="keyword">;<br><br></span><span class="default">var_export</span><span class="keyword">( </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$utf8</span><span class="keyword">) ); </span><span class="comment">// 59<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br></span><span class="default">var_export</span><span class="keyword">( </span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$utf8</span><span class="keyword">, </span><span class="string">&apos;utf8&apos;</span><span class="keyword">) ); </span><span class="comment">// 32<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The easiest way to determine the character count of a UTF8 string is to pass the text through utf8_decode() first:
<br>
<br><span class="default">&lt;?php
<br>$length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">utf8_decode</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>utf8_decode() converts characters that are not in ISO-8859-1 to &apos;?&apos;, which, for the purpose of counting, is quite alright.</span>
</div>
  

#


<div class="phpcode"><span class="html">
We just ran into what we thought was a bug but turned out to be a documented difference in behavior between PHP 5.2 &amp; 5.3.&#xA0; Take the following code example:<br><br><span class="default">&lt;?php<br><br>$attributes </span><span class="keyword">= array(</span><span class="string">&apos;one&apos;</span><span class="keyword">, </span><span class="string">&apos;two&apos;</span><span class="keyword">, </span><span class="string">&apos;three&apos;</span><span class="keyword">);<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$attributes</span><span class="keyword">) == </span><span class="default">0 </span><span class="keyword">&amp;&amp; !</span><span class="default">is_bool</span><span class="keyword">(</span><span class="default">$attributes</span><span class="keyword">)) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;We are in the &apos;if&apos;\n&quot;</span><span class="keyword">;&#xA0; </span><span class="comment">//&#xA0; PHP 5.3<br></span><span class="keyword">} else {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;We are in the &apos;else&apos;\n&quot;</span><span class="keyword">;&#xA0; </span><span class="comment">//&#xA0; PHP 5.2<br></span><span class="keyword">}<br><br></span><span class="default">?&gt;<br></span><br>This is because in 5.2 strlen will automatically cast anything passed to it as a string, and casting an array to a string yields the string &quot;Array&quot;.&#xA0; In 5.3, this changed, as noted in the following point in the backward incompatible changes in 5.3 (<a href="http://www.php.net/manual/en/migration53.incompatible.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/migration53.incompatible.php</a>):<br><br>&quot;The newer internal parameter parsing API has been applied across all the extensions bundled with PHP 5.3.x. This parameter parsing API causes functions to return NULL when passed incompatible parameters. There are some exceptions to this rule, such as the get_class() function, which will continue to return FALSE on error.&quot;<br><br>So, in PHP 5.3, strlen($attributes) returns NULL, while in PHP 5.2, strlen($attributes) returns the integer 5.&#xA0; This likely affects other functions, so if you are getting different behaviors or new bugs suddenly, check if you have upgraded to 5.3 (which we did recently), and then check for some warnings in your logs like this:<br><br>strlen() expects parameter 1 to be string, array given in /var/www/sis/lib/functions/advanced_search_lib.php on line 1028<br><br>If so, then you are likely experiencing this changed behavior.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I would like to demonstrate that you need more than just this function in order to truly test for an empty string. The reason being that <span class="default">&lt;?php strlen</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">); </span><span class="default">?&gt;</span> will return 0. So how do you know if the value was null, or truly an empty string?<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br></span><span class="default">$len </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">);<br></span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br><br>echo </span><span class="string">&quot;Length: &quot; </span><span class="keyword">. </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;Length: </span><span class="default">$len</span><span class="string"> &lt;br&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;Length: &quot; </span><span class="keyword">. </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">null</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">) === </span><span class="default">0</span><span class="keyword">) echo </span><span class="string">&apos;Null length is Zero &lt;br&gt;&apos;</span><span class="keyword">;<br>if (</span><span class="default">$len </span><span class="keyword">=== </span><span class="default">0</span><span class="keyword">) echo </span><span class="string">&apos;Null length is still Zero &lt;br&gt;&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">) == </span><span class="default">0 </span><span class="keyword">&amp;&amp; !</span><span class="default">is_null</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">)) echo </span><span class="string">&apos;!is_null(): $foo is truly an empty string &lt;br&gt;&apos;</span><span class="keyword">;<br>else echo </span><span class="string">&apos;!is_null(): $foo is probably null &lt;br&gt;&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">) == </span><span class="default">0 </span><span class="keyword">&amp;&amp; isset(</span><span class="default">$foo</span><span class="keyword">)) echo </span><span class="string">&apos;isset(): $foo is truly an empty string &lt;br&gt;&apos;</span><span class="keyword">;<br>else echo </span><span class="string">&apos;isset(): $foo is probably null &lt;br&gt;&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$bar</span><span class="keyword">) == </span><span class="default">0 </span><span class="keyword">&amp;&amp; !</span><span class="default">is_null</span><span class="keyword">(</span><span class="default">$bar</span><span class="keyword">)) echo </span><span class="string">&apos;!is_null(): $bar is truly an empty string &lt;br&gt;&apos;</span><span class="keyword">;<br>else echo </span><span class="string">&apos;!is_null(): $foo is probably null &lt;br&gt;&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$bar</span><span class="keyword">) == </span><span class="default">0 </span><span class="keyword">&amp;&amp; isset(</span><span class="default">$bar</span><span class="keyword">)) echo </span><span class="string">&apos;isset(): $bar is truly an empty string &lt;br&gt;&apos;</span><span class="keyword">;<br>else echo </span><span class="string">&apos;isset(): $foo is probably null &lt;br&gt;&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>// Begin Output:<br>Length: 0<br>Length: 0 <br>Length: 0<br><br>Null length is Zero <br>Null length is still Zero <br><br>!is_null(): $foo is probably null <br>isset(): $foo is probably null <br><br>!is_null(): $bar is truly an empty string <br>isset(): $bar is truly an empty string <br>// End Output<br><br>So it would seem you need either is_null() or isset() in addition to strlen() if you care whether or not the original value was null.</span>
</div>
  

#


<div class="phpcode"><span class="html">
PHP&apos;s strlen function behaves differently than the C strlen function in terms of its handling of null bytes (&apos;\0&apos;).&#xA0; <br><br>In PHP, a null byte in a string does NOT count as the end of the string, and any null bytes are included in the length of the string.<br><br>For example, in PHP:<br><br>strlen( &quot;te\0st&quot; ) = 5<br><br>In C, the same call would return 2.<br><br>Thus, PHP&apos;s strlen function can be used to find the number of bytes in a binary string (for example, binary data returned by base64_decode).</span>
</div>
  

#


<div class="phpcode"><span class="html">
Attention with utf8:<br>$foo = &quot;b&#xE4;r&quot;;<br>strlen($foo) will return 4 and not 3 as expected..</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strlen.php)

**[To root](/README.md)**