# strtr




<div class="phpcode"><span class="html">
Here&apos;s an important real-world example use-case for strtr where str_replace will not work or will introduce obscure bugs:<br><br><span class="default">&lt;?php<br><br>$strTemplate </span><span class="keyword">= </span><span class="string">&quot;My name is :name, not :name2.&quot;</span><span class="keyword">;<br></span><span class="default">$strParams </span><span class="keyword">= [<br>&#xA0; </span><span class="string">&apos;:name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Dave&apos;</span><span class="keyword">,<br>&#xA0; </span><span class="string">&apos;Dave&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;:name2 or :password&apos;</span><span class="keyword">, </span><span class="comment">// a wrench in the otherwise sensible input<br>&#xA0; </span><span class="string">&apos;:name2&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Steve&apos;</span><span class="keyword">,<br>&#xA0; </span><span class="string">&apos;:pass&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;7hf2348&apos;</span><span class="keyword">, </span><span class="comment">// sensitive data that maybe shouldn&apos;t be here<br></span><span class="keyword">];<br><br>echo </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$strTemplate</span><span class="keyword">, </span><span class="default">$strParams</span><span class="keyword">);<br></span><span class="comment">// &quot;My name is Dave, not Steve.&quot;<br><br></span><span class="keyword">echo </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$strParams</span><span class="keyword">), </span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$strParams</span><span class="keyword">), </span><span class="default">$strTemplate</span><span class="keyword">);<br></span><span class="comment">// &quot;My name is Steve or 7hf2348word, not Steve or 7hf2348word2.&quot;<br><br></span><span class="default">?&gt;<br></span><br>Any time you&apos;re trying to template out a string and don&apos;t necessarily know what the replacement keys/values will be (or fully understand the implications of and control their content and order), str_replace will introduce the potential to incorrectly match your keys because it does not expand the longest keys first.<br><br>Further, str_replace will replace in previous replacements, introducing potential for unintended nested expansions.&#xA0; Doing so can put the wrong data into the &quot;sub-template&quot; or even give users a chance to provide input that exposes data (if they get to define some of the replacement strings).<br><br>Don&apos;t support recursive expansion unless you need it and know it will be safe.&#xA0; When you do support it, do so explicitly by repeating strtr calls until no more expansions are occurring or a sane iteration limit is reached, so that the results never implicitly depend on order of your replacement keys.&#xA0; Also make certain that any user input will expanded in an isolated step after any sensitive data is already expanded into the output and no longer available as input.<br><br>Note: using some character(s) around your keys to designate them also reduces the possibility of unintended mangling of output, whether maliciously triggered or otherwise.&#xA0; Thus the use of a colon prefix in these examples, which you can easily enforce when accepting replacement input to your templating/translation system.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Since strtr (like PHP&apos;s other string functions) treats strings as a sequence of bytes, and since UTF-8 and other multibyte encodings use - by definition - more than one byte for at least some characters, the three-string form is likely to have problems. Use the associative array form to specify the mapping.<br><br><span class="default">&lt;?php<br></span><span class="comment">// Assuming UTF-8<br></span><span class="default">$str </span><span class="keyword">= </span><span class="string">&apos;&#xC4;bc &#xC4;bc&apos;</span><span class="keyword">; </span><span class="comment">// strtr() sees this as nine bytes (including two for each &#xC4;)<br></span><span class="keyword">echo </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="string">&apos;&#xC4;&apos;</span><span class="keyword">, </span><span class="string">&apos;a&apos;</span><span class="keyword">); </span><span class="comment">// The second argument is equivalent to the string &quot;\xc3\x84&quot; so &quot;\xc3&quot; gets replaced by &quot;a&quot; and the &quot;\x84&quot; is ignored<br><br></span><span class="keyword">echo </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, array(</span><span class="string">&apos;&#xC4;&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;a&apos;</span><span class="keyword">)); </span><span class="comment">// Works much better<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
fixed &quot;normaliza&quot; functions written below to include Slavic Latin characters... also, it doesn&apos;t return lowercase any more (you can easily get that by applying strtolower yourself)...<br><br>also, renamed to normalize()<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">normalize </span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$table </span><span class="keyword">= array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#x160;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;S&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x161;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;s&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x110;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;Dj&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x111;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;dj&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x17D;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;Z&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x17E;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;z&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x10C;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;C&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x10D;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;c&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x106;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;C&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x107;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;c&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xC0;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC1;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC2;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC3;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC4;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC5;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC6;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC7;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;C&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC8;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;E&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xC9;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;E&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xCA;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;E&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xCB;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;E&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xCC;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;I&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xCD;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;I&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xCE;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;I&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xCF;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;I&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD1;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;N&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD2;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;O&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD3;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;O&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD4;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;O&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xD5;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;O&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD6;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;O&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD8;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;O&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xD9;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;U&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xDA;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;U&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xDB;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;U&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xDC;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;U&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xDD;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;Y&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xDE;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;B&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xDF;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;Ss&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xE0;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE1;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE2;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE3;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE4;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE5;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE6;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE7;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;c&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE8;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;e&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xE9;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;e&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xEA;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;e&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xEB;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;e&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xEC;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xED;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xEE;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xEF;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF0;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF1;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;n&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF2;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF3;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xF4;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF5;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF6;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF8;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;o&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xF9;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;u&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xFA;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;u&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xFB;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;u&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xFD;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;y&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xFD;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;y&apos;</span><span class="keyword">, </span><span class="string">&apos;&#xFE;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;b&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&#xFF;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;y&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x154;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;R&apos;</span><span class="keyword">, </span><span class="string">&apos;&#x155;&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;r&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; );<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$table</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
OK, I debugged the function (had some errors)<br>Here it is:<br><br>if(!function_exists(&quot;stritr&quot;)){<br>&#xA0; &#xA0; function stritr($string, $one = NULL, $two = NULL){<br>/*<br>stritr - case insensitive version of strtr<br>Author: Alexander Peev<br>Posted in PHP.NET<br>*/<br>&#xA0; &#xA0; &#xA0; &#xA0; if(&#xA0; is_string( $one )&#xA0; ){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $two = strval( $two );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $one = substr(&#xA0; $one, 0, min( strlen($one), strlen($two) )&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $two = substr(&#xA0; $two, 0, min( strlen($one), strlen($two) )&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $product = strtr(&#xA0; $string, ( strtoupper($one) . strtolower($one) ), ( $two . $two )&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $product;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; else if(&#xA0; is_array( $one )&#xA0; ){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $pos1 = 0;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $product = $string;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while(&#xA0; count( $one ) &gt; 0&#xA0; ){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $positions = array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(&#xA0; $one as $from =&gt; $to&#xA0; ){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(&#xA0;&#xA0; (&#xA0; $pos2 = stripos( $product, $from, $pos1 )&#xA0; ) === FALSE&#xA0;&#xA0; ){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(&#xA0; $one[ $from ]&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $positions[ $from ] = $pos2;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(&#xA0; count( $one ) &lt;= 0&#xA0; )break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $winner = min( $positions );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = array_search(&#xA0; $winner, $positions&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $product = (&#xA0;&#xA0; substr(&#xA0; $product, 0, $winner&#xA0; ) . $one[$key] . substr(&#xA0; $product, ( $winner + strlen($key) )&#xA0; )&#xA0;&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $pos1 = (&#xA0; $winner + strlen( $one[$key] )&#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $product;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $string;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }/* endfunction stritr */<br>}/* endfunction exists stritr */</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strtr.php)

**[To root](/README.md)**