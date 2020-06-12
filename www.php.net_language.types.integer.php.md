# Integers




<div class="phpcode"><span class="html">
A leading zero in a numeric literal means &quot;this is octal&quot;. But don&apos;t be confused: a leading zero in a string does not. Thus:<br>$x = 0123;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // 83<br>$y = &quot;0123&quot; + 0&#xA0; &#xA0;&#xA0; // 123</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here are some tricks to convert from a &quot;dotted&quot; IP address to a LONG int, and backwards. This is very useful because accessing an IP addy in a database table is very much faster if it&apos;s stored as a BIGINT rather than in characters.<br><br>IP to BIGINT:<br><span class="default">&lt;?php<br>&#xA0; $ipArr&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">,</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REMOTE_ADDR&apos;</span><span class="keyword">]);<br>&#xA0; </span><span class="default">$ip&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] * </span><span class="default">0x1000000<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">+ </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] * </span><span class="default">0x10000<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">+ </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] * </span><span class="default">0x100<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">+ </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ;<br></span><span class="default">?&gt;<br></span><br>IP as BIGINT read from db back to dotted form:<br><br>Keep in mind, PHP integer operators are INTEGER -- not long. Also, since there is no integer divide in PHP, we save a couple of S-L-O-W floor (&lt;division&gt;)&apos;s by doing bitshifts. We must use floor(/) for $ipArr[0] because though $ipVal is stored as a long value, $ipVal &gt;&gt; 24 will operate on a truncated, integer value of $ipVal! $ipVint is, however, a nice integer, so <br>we can enjoy the bitshifts.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0; $ipVal </span><span class="keyword">= </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;client_IP&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ipArr </span><span class="keyword">= array(</span><span class="default">0 </span><span class="keyword">=&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">floor</span><span class="keyword">(&#xA0; </span><span class="default">$ipVal&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">/ </span><span class="default">0x1000000</span><span class="keyword">) );<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ipVint&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">$ipVal</span><span class="keyword">-(</span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]*</span><span class="default">0x1000000</span><span class="keyword">); </span><span class="comment">// for clarity<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = (</span><span class="default">$ipVint </span><span class="keyword">&amp; </span><span class="default">0xFF0000</span><span class="keyword">)&#xA0; &gt;&gt; </span><span class="default">16</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = (</span><span class="default">$ipVint </span><span class="keyword">&amp; </span><span class="default">0xFF00&#xA0; </span><span class="keyword">)&#xA0; &gt;&gt; </span><span class="default">8</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ipArr</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] =&#xA0; </span><span class="default">$ipVint </span><span class="keyword">&amp; </span><span class="default">0xFF</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ipDotted </span><span class="keyword">= </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">, </span><span class="default">$ipArr</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
-------------------------------------------------------------------------<br>Question : <br>var_dump((int) 010);&#xA0; //Output 8<br><br>var_dump((int) &quot;010&quot;); //output 10<br><br>First one is octal notation so the output is correct. But what about the when converting &quot;010&quot; to integer. it should be also output 8 ?<br>--------------------------------------------------------------------------<br>Answer :<br><br>Casting to an integer using (int) will always cast to the default base, which is 10.<br><br>Casting a string to a number this way does not take into account the many ways of formatting an integer value in PHP (leading zero for base 8, leading &quot;0x&quot; for base 16, leading &quot;0b&quot; for base 2). It will simply look at the first characters in a string and convert them to a base 10 integer. Leading zeroes will be stripped off because they have no meaning in numerical values, so you will end up with the decimal value 10 for (int)&quot;010&quot;.<br><br>Converting an integer value between bases using (int)010 will take into account the various ways of formatting an integer. A leading zero like in 010 means the number is in octal notation, using (int)010 will convert it to the decimal value 8 in base 10.<br><br>This is similar to how you use 0x10 to write in hexadecimal (base 16) notation. Using (int)0x10 will convert that to the base 10 decimal value 16, whereas using (int)&quot;0x10&quot; will end up with the decimal value 0: since the &quot;x&quot; is not a numerical value, anything after that will be ignored.<br><br>If you want to interpret the string &quot;010&quot; as an octal value, you need to instruct PHP to do so. intval(&quot;010&quot;, 8) will interpret the number in base 8 instead of the default base 10, and you will end up with the decimal value 8. You could also use octdec(&quot;010&quot;) to convert the octal string to the decimal value 8. Another option is to use base_convert(&quot;010&quot;, 8, 10) to explicitly convert the number &quot;010&quot; from base 8 to base 10, however this function will return the string &quot;8&quot; instead of the integer 8.<br><br>Casting a string to an integer follows the same the logic used by the intval function:<br><br>Returns the integer value of var, using the specified base for the conversion (the default is base 10).<br>intval allows specifying a different base as the second argument, whereas a straight cast operation does not, so using (int) will always treat a string as being in base 10.<br><br>php &gt; var_export((int) &quot;010&quot;);<br>10<br>php &gt; var_export(intval(&quot;010&quot;));<br>10<br>php &gt; var_export(intval(&quot;010&quot;, 8));<br>8</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;There is no integer division operator in PHP&quot;. But since PHP 7, there is the intdiv function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Converting to an integer works only if the input begins with a number<br>(int) &quot;5txt&quot; // will output the integer 5<br>(int) &quot;before5txt&quot; // will output the integer 0<br>(int) &quot;53txt&quot; // will output the integer 53<br>(int) &quot;53txt534text&quot; // will output the integer 53</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful with using the modulo operation on big numbers, it will cast a float argument to an int and may return wrong results. For example:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; $i </span><span class="keyword">= </span><span class="default">6887129852</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="string">&quot;i=</span><span class="default">$i</span><span class="string">\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="string">&quot;i%36=&quot;</span><span class="keyword">.(</span><span class="default">$i</span><span class="keyword">%</span><span class="default">36</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="string">&quot;alternative i%36=&quot;</span><span class="keyword">.(</span><span class="default">$i</span><span class="keyword">-</span><span class="default">floor</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">/</span><span class="default">36</span><span class="keyword">)*</span><span class="default">36</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>Will output:<br>i=6.88713E+009<br>i%36=-24<br>alternative i%36=20</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.integer.php)

**[To root](/)**