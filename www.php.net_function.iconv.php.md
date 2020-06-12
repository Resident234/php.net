# iconv




<div class="phpcode"><span class="html">
The &quot;//ignore&quot; option doesn&apos;t work with recent versions of the iconv library.&#xA0; So if you&apos;re having trouble with that option, you aren&apos;t alone.&#xA0; 
<br>
<br>That means you can&apos;t currently use this function to filter invalid characters.&#xA0; Instead it silently fails and returns an empty string (or you&apos;ll get a notice but only if you have E_NOTICE enabled).
<br>
<br>This has been a known bug with a known solution for at least since 2009 years but no one seems to be willing to fix it (PHP must pass the -c option to iconv).&#xA0; It&apos;s still broken as of the latest release 5.4.3. 
<br>
<br><a href="https://bugs.php.net/bug.php?id=48147" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=48147</a>
<br><a href="https://bugs.php.net/bug.php?id=52211" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=52211</a>
<br><a href="https://bugs.php.net/bug.php?id=61484" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=61484</a>
<br>
<br>[UPDATE 15-JUN-2012]
<br>Here&apos;s a workaround...
<br>
<br>&#xA0; ini_set(&apos;mbstring.substitute_character&apos;, &quot;none&quot;);
<br>&#xA0; $text= mb_convert_encoding($text, &apos;UTF-8&apos;, &apos;UTF-8&apos;);
<br>
<br>That will strip invalid characters from UTF-8 strings (so that you can insert it into a database, etc.).&#xA0; Instead of &quot;none&quot; you can also use the value 32 if you want it to insert spaces in place of the invalid characters.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that iconv(&apos;UTF-8&apos;, &apos;ASCII//TRANSLIT&apos;, ...) doesn&apos;t work properly when locale category LC_CTYPE is set to C or POSIX. You must choose another locale otherwise all non-ASCII characters will be replaced with question marks. This is at least true with glibc 2.5.<br><br>Example:<br><span class="default">&lt;?php<br>setlocale</span><span class="keyword">(</span><span class="default">LC_CTYPE</span><span class="keyword">, </span><span class="string">&apos;POSIX&apos;</span><span class="keyword">);<br>echo </span><span class="default">iconv</span><span class="keyword">(</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="string">&apos;ASCII//TRANSLIT&apos;</span><span class="keyword">, </span><span class="string">&quot;&#x17D;lu&#x165;ou&#x10D;k&#xFD; k&#x16F;&#x148;\n&quot;</span><span class="keyword">);<br></span><span class="comment">// ?lu?ou?k? k??<br><br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_CTYPE</span><span class="keyword">, </span><span class="string">&apos;cs_CZ&apos;</span><span class="keyword">);<br>echo </span><span class="default">iconv</span><span class="keyword">(</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="string">&apos;ASCII//TRANSLIT&apos;</span><span class="keyword">, </span><span class="string">&quot;&#x17D;lu&#x165;ou&#x10D;k&#xFD; k&#x16F;&#x148;\n&quot;</span><span class="keyword">);<br></span><span class="comment">// Zlutoucky kun<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Interestingly, setting different target locales results in different, yet appropriate, transliterations. For example:<br><br><span class="default">&lt;?php<br></span><span class="comment">//some German<br></span><span class="default">$utf8_sentence </span><span class="keyword">= </span><span class="string">&apos;Wei&#xDF;, Goldmann, G&#xF6;bel, Weiss, G&#xF6;the, Goethe und G&#xF6;tz&apos;</span><span class="keyword">;<br><br></span><span class="comment">//UK<br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="string">&apos;en_GB&apos;</span><span class="keyword">);<br><br></span><span class="comment">//transliterate<br></span><span class="default">$trans_sentence </span><span class="keyword">= </span><span class="default">iconv</span><span class="keyword">(</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="string">&apos;ASCII//TRANSLIT&apos;</span><span class="keyword">, </span><span class="default">$utf8_sentence</span><span class="keyword">);<br><br></span><span class="comment">//gives [Weiss, Goldmann, Gobel, Weiss, Gothe, Goethe und Gotz]<br>//which is our original string flattened into 7-bit ASCII as<br>//an English speaker would do it (ie. simply remove the umlauts)<br></span><span class="keyword">echo </span><span class="default">$trans_sentence </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">;<br><br></span><span class="comment">//Germany<br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="string">&apos;de_DE&apos;</span><span class="keyword">);<br><br></span><span class="default">$trans_sentence </span><span class="keyword">= </span><span class="default">iconv</span><span class="keyword">(</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="string">&apos;ASCII//TRANSLIT&apos;</span><span class="keyword">, </span><span class="default">$utf8_sentence</span><span class="keyword">);<br><br></span><span class="comment">//gives [Weiss, Goldmann, Goebel, Weiss, Goethe, Goethe und Goetz]<br>//which is exactly how a German would transliterate those<br>//umlauted characters if forced to use 7-bit ASCII!<br>//(because really &#xE4; = ae, &#xF6; = oe and &#xFC; = ue)<br></span><span class="keyword">echo </span><span class="default">$trans_sentence </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
to test different combinations of convertions between charsets (when we don&apos;t know the source charset and what is the convenient destination charset) this is an example :
<br>
<br><span class="default">&lt;?php
<br>$tab </span><span class="keyword">= array(</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">, </span><span class="string">&quot;ASCII&quot;</span><span class="keyword">, </span><span class="string">&quot;Windows-1252&quot;</span><span class="keyword">, </span><span class="string">&quot;ISO-8859-15&quot;</span><span class="keyword">, </span><span class="string">&quot;ISO-8859-1&quot;</span><span class="keyword">, </span><span class="string">&quot;ISO-8859-6&quot;</span><span class="keyword">, </span><span class="string">&quot;CP1256&quot;</span><span class="keyword">);
<br></span><span class="default">$chain </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>foreach (</span><span class="default">$tab </span><span class="keyword">as </span><span class="default">$i</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$tab </span><span class="keyword">as </span><span class="default">$j</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$chain </span><span class="keyword">.= </span><span class="string">&quot; </span><span class="default">$i$j</span><span class="string"> &quot;</span><span class="keyword">.</span><span class="default">iconv</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">, </span><span class="default">$j</span><span class="keyword">, </span><span class="string">&quot;</span><span class="default">$my_string</span><span class="string">&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>echo </span><span class="default">$chain</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>then after displaying, you use the $i$j that shows good displaying.
<br>NB: you can add other charsets to $tab&#xA0; to test other cases.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are getting question-marks in your iconv output when transliterating, be sure to &apos;setlocale&apos; to something your system supports.<br><br>Some PHP CMS&apos;s will default setlocale to &apos;C&apos;, this can be a problem.<br><br>use the &quot;locale&quot; command to find out a list..<br><br>$ locale -a<br>C<br>en_AU.utf8<br>POSIX<br><br><span class="default">&lt;?php<br>&#xA0; setlocale</span><span class="keyword">(</span><span class="default">LC_CTYPE</span><span class="keyword">, </span><span class="string">&apos;en_AU.utf8&apos;</span><span class="keyword">);<br>&#xA0; </span><span class="default">$str </span><span class="keyword">= </span><span class="default">iconv</span><span class="keyword">(</span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="string">&apos;ASCII//TRANSLIT&apos;</span><span class="keyword">, </span><span class="string">&quot;C&#xF4;te d&apos;Ivoire&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Like many other people, I have encountered massive problems when using iconv() to convert between encodings (from UTF-8 to ISO-8859-15 in my case), especially on large strings.<br><br>The main problem here is that when your string contains illegal UTF-8 characters, there is no really straight forward way to handle those. iconv() simply (and silently!) terminates the string when encountering the problematic characters (also if using //IGNORE), returning a clipped string. The<br><br><span class="default">&lt;?php<br><br>$newstring </span><span class="keyword">= </span><span class="default">html_entity_decode</span><span class="keyword">(</span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$oldstring</span><span class="keyword">, </span><span class="default">ENT_QUOTES</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">), </span><span class="default">ENT_QUOTES </span><span class="keyword">, </span><span class="string">&apos;ISO-8859-15&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>workaround suggested here and elsewhere will also break when encountering illegal characters, at least dropping a useful note (&quot;htmlentities(): Invalid multibyte sequence in argument in...&quot;)<br><br>I have found a lot of hints, suggestions and alternative methods (it&apos;s scary and in my opinion no good sign how many ways PHP natively provides to convert the encoding of strings), but none of them really worked, except for this one:<br><br><span class="default">&lt;?php<br><br>$newstring </span><span class="keyword">= </span><span class="default">mb_convert_encoding</span><span class="keyword">(</span><span class="default">$oldstring</span><span class="keyword">, </span><span class="string">&apos;ISO-8859-15&apos;</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There may be situations when a new version of a web site, all in UTF-8, has to display some old data remaining in the database with ISO-8859-1 accents. The problem is iconv(&quot;ISO-8859-1&quot;, &quot;UTF-8&quot;, $string) should not be applied if $string is already UTF-8 encoded.<br><br>I use this function that does&apos;nt need any extension :<br><br>function convert_utf8( $string ) { <br>&#xA0; &#xA0; if ( strlen(utf8_decode($string)) == strlen($string) ) {&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; // $string is not UTF-8<br>&#xA0; &#xA0; &#xA0; &#xA0; return iconv(&quot;ISO-8859-1&quot;, &quot;UTF-8&quot;, $string);<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; // already UTF-8<br>&#xA0; &#xA0; &#xA0; &#xA0; return $string;<br>&#xA0; &#xA0; }<br>}<br><br> I have not tested it extensively, hope it may help.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I have used iconv to convert from cp1251 into UTF-8. I spent a day to investigate why a string with Russian capital &apos;&#x420;&apos; (sounds similar to &apos;r&apos;) at the end cannot be inserted into a database.<br><br>The problem is not in iconv. But &apos;&#x420;&apos; in cp1251 is chr(208) and &apos;&#x420;&apos; in UTF-8 is chr(208).chr(106). chr(106) is one of the space symbol which match &apos;\s&apos; in regex. So, it can be taken by a greedy &apos;+&apos; or &apos;*&apos; operator. In that case, you loose &apos;&#x420;&apos; in your string.<br><br>For example, &apos;&#x413;&#x420;&#xA0;&#xA0; &apos; (Russian, UTF-8). Function preg_match. Regex is &apos;(.+?)[\s]*&apos;. Then &apos;(.+?)&apos; matches &apos;&#x413;&apos;.chr(208) and &apos;[\s]*&apos; matches chr(106).&apos;&#xA0;&#xA0; &apos;.<br><br>Although, it is not a bug of iconv, but it looks like it very much. That&apos;s why I put this comment here.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.iconv.php)

**[⬆ to root](/)**