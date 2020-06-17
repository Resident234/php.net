# mb_convert_case




<div class="phpcode"><span class="html">
as the previouly posted version of this function doesn&apos;t handle UTF-8 characters, I simply tried to replace ucfirst to mb_convert_case, but then any previous case foldings were lost while looping through delimiters. <br>So I decided to do an mb_convert_case on the input string (it also deals with words is uppercase wich may also be problematic when doing case-sensitive search), and do the rest of checking after that.<br><br>As with mb_convert_case, words are capitalized, I also added lowercase convertion for the exceptions, but, for the above mentioned reason, I left ucfirst unchanged.<br><br>Now it works fine for utf-8 strings as well, except for string delimiters followed by an UTF-8 character (&quot;Mc&#xE1;d&#xE1;m&quot; is unchanged, while &quot;mcdunno&apos;s&quot; is converted to &quot;McDunno&apos;s&quot; and &quot;&#xF6;kr&#xF6;s-T&#xD3;TH &#xE9;DUa&quot; in also put in the correct form)<br><br>I use it for checking user input on names and addresses, so exceptions list contains some hungarian words too.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">titleCase</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$delimiters </span><span class="keyword">= array(</span><span class="string">&quot; &quot;</span><span class="keyword">, </span><span class="string">&quot;-&quot;</span><span class="keyword">, </span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="string">&quot;&apos;&quot;</span><span class="keyword">, </span><span class="string">&quot;O&apos;&quot;</span><span class="keyword">, </span><span class="string">&quot;Mc&quot;</span><span class="keyword">), </span><span class="default">$exceptions </span><span class="keyword">= array(</span><span class="string">&quot;&#xFA;t&quot;</span><span class="keyword">, </span><span class="string">&quot;u&quot;</span><span class="keyword">, </span><span class="string">&quot;s&quot;</span><span class="keyword">, </span><span class="string">&quot;&#xE9;s&quot;</span><span class="keyword">, </span><span class="string">&quot;utca&quot;</span><span class="keyword">, </span><span class="string">&quot;t&#xE9;r&quot;</span><span class="keyword">, </span><span class="string">&quot;krt&quot;</span><span class="keyword">, </span><span class="string">&quot;k&#xF6;r&#xFA;t&quot;</span><span class="keyword">, </span><span class="string">&quot;s&#xE9;t&#xE1;ny&quot;</span><span class="keyword">, </span><span class="string">&quot;I&quot;</span><span class="keyword">, </span><span class="string">&quot;II&quot;</span><span class="keyword">, </span><span class="string">&quot;III&quot;</span><span class="keyword">, </span><span class="string">&quot;IV&quot;</span><span class="keyword">, </span><span class="string">&quot;V&quot;</span><span class="keyword">, </span><span class="string">&quot;VI&quot;</span><span class="keyword">, </span><span class="string">&quot;VII&quot;</span><span class="keyword">, </span><span class="string">&quot;VIII&quot;</span><span class="keyword">, </span><span class="string">&quot;IX&quot;</span><span class="keyword">, </span><span class="string">&quot;X&quot;</span><span class="keyword">, </span><span class="string">&quot;XI&quot;</span><span class="keyword">, </span><span class="string">&quot;XII&quot;</span><span class="keyword">, </span><span class="string">&quot;XIII&quot;</span><span class="keyword">, </span><span class="string">&quot;XIV&quot;</span><span class="keyword">, </span><span class="string">&quot;XV&quot;</span><span class="keyword">, </span><span class="string">&quot;XVI&quot;</span><span class="keyword">, </span><span class="string">&quot;XVII&quot;</span><span class="keyword">, </span><span class="string">&quot;XVIII&quot;</span><span class="keyword">, </span><span class="string">&quot;XIX&quot;</span><span class="keyword">, </span><span class="string">&quot;XX&quot;</span><span class="keyword">, </span><span class="string">&quot;XXI&quot;</span><span class="keyword">, </span><span class="string">&quot;XXII&quot;</span><span class="keyword">, </span><span class="string">&quot;XXIII&quot;</span><span class="keyword">, </span><span class="string">&quot;XXIV&quot;</span><span class="keyword">, </span><span class="string">&quot;XXV&quot;</span><span class="keyword">, </span><span class="string">&quot;XXVI&quot;</span><span class="keyword">, </span><span class="string">&quot;XXVII&quot;</span><span class="keyword">, </span><span class="string">&quot;XXVIII&quot;</span><span class="keyword">, </span><span class="string">&quot;XXIX&quot;</span><span class="keyword">, </span><span class="string">&quot;XXX&quot; </span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">/*<br>&#xA0; &#xA0; &#xA0; &#xA0; * Exceptions in lower case are words you don&apos;t want converted<br>&#xA0; &#xA0; &#xA0; &#xA0; * Exceptions all in upper case are any words you don&apos;t want converted to title case<br>&#xA0; &#xA0; &#xA0; &#xA0; *&#xA0;&#xA0; but should be converted to upper case, e.g.:<br>&#xA0; &#xA0; &#xA0; &#xA0; *&#xA0;&#xA0; king henry viii or king henry Viii should be King Henry VIII<br>&#xA0; &#xA0; &#xA0; &#xA0; */<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">mb_convert_case</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">MB_CASE_TITLE</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0;&#xA0; foreach (</span><span class="default">$delimiters </span><span class="keyword">as </span><span class="default">$dlnr </span><span class="keyword">=&gt; </span><span class="default">$delimiter</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$words </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$newwords </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; foreach (</span><span class="default">$words </span><span class="keyword">as </span><span class="default">$wordnr </span><span class="keyword">=&gt; </span><span class="default">$word</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">), </span><span class="default">$exceptions</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// check exceptions list for any words that should be in upper case<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$word </span><span class="keyword">= </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; elseif (</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">mb_strtolower</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">), </span><span class="default">$exceptions</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// check exceptions list for any words that should be in upper case<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$word </span><span class="keyword">= </span><span class="default">mb_strtolower</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; elseif (!</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">, </span><span class="default">$exceptions</span><span class="keyword">) ){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// convert to uppercase (non-utf8 only)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$word </span><span class="keyword">= </span><span class="default">ucfirst</span><span class="keyword">(</span><span class="default">$word</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$newwords</span><span class="keyword">, </span><span class="default">$word</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">join</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$newwords</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0;&#xA0; }</span><span class="comment">//foreach<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">return </span><span class="default">$string</span><span class="keyword">;<br>} <br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-convert-case.php)

**[To root](/README.md)**