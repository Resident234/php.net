# base64_encode




<div class="phpcode"><span class="html">
For anyone interested in the &apos;base64url&apos; variant encoding, you can use this pair of functions:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">base64url_encode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) {
<br>&#xA0; return </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">strtr</span><span class="keyword">(</span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">), </span><span class="string">&apos;+/&apos;</span><span class="keyword">, </span><span class="string">&apos;-_&apos;</span><span class="keyword">), </span><span class="string">&apos;=&apos;</span><span class="keyword">);
<br>}
<br>
<br>function </span><span class="default">base64url_decode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) {
<br>&#xA0; return </span><span class="default">base64_decode</span><span class="keyword">(</span><span class="default">str_pad</span><span class="keyword">(</span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">, </span><span class="string">&apos;-_&apos;</span><span class="keyword">, </span><span class="string">&apos;+/&apos;</span><span class="keyword">), </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) % </span><span class="default">4</span><span class="keyword">, </span><span class="string">&apos;=&apos;</span><span class="keyword">, </span><span class="default">STR_PAD_RIGHT</span><span class="keyword">));
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In PHP 7, the padding issue with base64_decode() is no more - the following is totally fine:<br><br>function base64_encode_url($string) {<br>&#xA0; &#xA0; return str_replace([&apos;+&apos;,&apos;/&apos;,&apos;=&apos;], [&apos;-&apos;,&apos;_&apos;,&apos;&apos;], base64_encode($string));<br>}<br><br>function base64_decode_url($string) {<br>&#xA0; &#xA0; return base64_decode(str_replace([&apos;-&apos;,&apos;_&apos;], [&apos;+&apos;,&apos;/&apos;], $string));<br>}<br><br>Checked here with random_bytes() and random lengths:<br><br><a href="https://3v4l.org/aEs4o" rel="nofollow" target="_blank">https://3v4l.org/aEs4o</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
gutzmer at usa dot net&apos;s ( <a href="http://php.net/manual/en/function.base64-encode.php#103849" rel="nofollow" target="_blank">http://php.net/manual/en/function.base64-encode.php#103849</a> ) base64url_decode() function doesn&apos;t pad longer strings with &apos;=&apos;s. Here is a corrected version: <br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">base64url_encode</span><span class="keyword">( </span><span class="default">$data </span><span class="keyword">){<br>&#xA0; return </span><span class="default">rtrim</span><span class="keyword">( </span><span class="default">strtr</span><span class="keyword">( </span><span class="default">base64_encode</span><span class="keyword">( </span><span class="default">$data </span><span class="keyword">), </span><span class="string">&apos;+/&apos;</span><span class="keyword">, </span><span class="string">&apos;-_&apos;</span><span class="keyword">), </span><span class="string">&apos;=&apos;</span><span class="keyword">);<br>}<br><br>function </span><span class="default">base64url_decode</span><span class="keyword">( </span><span class="default">$data </span><span class="keyword">){<br>&#xA0; return </span><span class="default">base64_decode</span><span class="keyword">( </span><span class="default">strtr</span><span class="keyword">( </span><span class="default">$data</span><span class="keyword">, </span><span class="string">&apos;-_&apos;</span><span class="keyword">, </span><span class="string">&apos;+/&apos;</span><span class="keyword">) . </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&apos;=&apos;</span><span class="keyword">, </span><span class="default">3 </span><span class="keyword">- ( </span><span class="default">3 </span><span class="keyword">+ </span><span class="default">strlen</span><span class="keyword">( </span><span class="default">$data </span><span class="keyword">)) % </span><span class="default">4 </span><span class="keyword">));<br>}<br><br></span><span class="comment">// proof<br></span><span class="keyword">for( </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">, </span><span class="default">$s </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">24</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">, </span><span class="default">$s </span><span class="keyword">.= </span><span class="default">substr</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$i</span><span class="string">&quot;</span><span class="keyword">, -</span><span class="default">1 </span><span class="keyword">)){<br>&#xA0; </span><span class="default">$base64_encoded&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">base64_encode</span><span class="keyword">(&#xA0; &#xA0; </span><span class="default">$s </span><span class="keyword">);<br>&#xA0; </span><span class="default">$base64url_encoded </span><span class="keyword">= </span><span class="default">base64url_encode</span><span class="keyword">( </span><span class="default">$s </span><span class="keyword">);<br>&#xA0; </span><span class="default">$base64url_decoded </span><span class="keyword">= </span><span class="default">base64url_decode</span><span class="keyword">( </span><span class="default">$base64url_encoded </span><span class="keyword">);<br>&#xA0; </span><span class="default">$base64_restored&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">strtr</span><span class="keyword">( </span><span class="default">$base64url_encoded</span><span class="keyword">, </span><span class="string">&apos;-_&apos;</span><span class="keyword">, </span><span class="string">&apos;+/&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; . </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&apos;=&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">3 </span><span class="keyword">- ( </span><span class="default">3 </span><span class="keyword">+ </span><span class="default">strlen</span><span class="keyword">( </span><span class="default">$base64url_encoded </span><span class="keyword">)) % </span><span class="default">4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">);<br>&#xA0; echo </span><span class="string">&quot;</span><span class="default">$s</span><span class="string">&lt;br&gt;</span><span class="default">$base64url_decoded</span><span class="string">&lt;br&gt;</span><span class="default">$base64_encoded</span><span class="string">&lt;br&gt;</span><span class="default">$base64_restored</span><span class="string">&lt;br&gt;</span><span class="default">$base64url_encoded</span><span class="string">&lt;br&gt;&lt;br&gt;&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Base64 encoding of large files.<br><br>Base64 encoding converts triples of eight-bit symbols into quadruples of six-bit symbols. Reading the input file in chunks that are a multiple of three bytes in length results in a chunk that can be encoded independently of the rest of the input file. MIME additionally enforces a line length of 76 characters plus the CRLF. 76 characters is enough for 19 quadruples of six-bit symbols thus representing 19 triples of eight-bit symbols. Reading 57 eight-bit symbols provides exactly enough data for a complete MIME-formatted line. Finally, PHP&apos;s default buffer size is 8192 bytes - enough for 143 MIME lines&apos; worth of input.<br><br>So if you read from the input file in chunks of 8151 (=57*143) bytes you will get (up to) 8151 eight-bit symbols, which encode as exactly 10868 six-bit symbols, which then wrap to exactly 143 MIME-formatted lines. There is no need to retain left-over symbols (either six- or eight-bit) from one chunk to the next. Just read a chunk, encode it, write it out, and go on to the next chunk. Obviously the last chunk will probably be shorter, but encoding it is still independent of the rest.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">while(!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$input_file</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; </span><span class="default">$plain </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$input_file</span><span class="keyword">, </span><span class="default">57 </span><span class="keyword">* </span><span class="default">143</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$encoded </span><span class="keyword">= </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$plain</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$encoded </span><span class="keyword">= </span><span class="default">chunk_split</span><span class="keyword">(</span><span class="default">$encoded</span><span class="keyword">, </span><span class="default">76</span><span class="keyword">, </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$output_file</span><span class="keyword">, </span><span class="default">$encoded</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;<br></span><br>Conversely, each 76-character MIME-formatted line (not counting the trailing CRLF) contains exactly enough data for 57 bytes of output without needing to retain leftover bits that need prepending to the next line. What that means is that each line can be decoded independently of the others, and the decoded chunks can then be concatenated together or written out sequentially. However, this does make the assumption that the encoded data really is MIME-formatted; without that assurance it is necessary to accept that the base64 data won&apos;t be so conveniently arranged.</span>
</div>
  

#


<div class="phpcode"><span class="html">
A function I&apos;m using to return local images as base64 encrypted code, i.e. embedding the image source into the html request.<br><br>This will greatly reduce your page load time as the browser will only need to send one server request for the entire page, rather than multiple requests for the HTML and the images. Requests need to be uploaded and 99% of the world are limited on their upload speed to the server.<br><br><span class="default">&lt;?php <br></span><span class="keyword">function </span><span class="default">base64_encode_image </span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">=</span><span class="default">string</span><span class="keyword">,</span><span class="default">$filetype</span><span class="keyword">=</span><span class="default">string</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">$filename</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$imgbinary </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">), </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;data:image/&apos; </span><span class="keyword">. </span><span class="default">$filetype </span><span class="keyword">. </span><span class="string">&apos;;base64,&apos; </span><span class="keyword">. </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$imgbinary</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>used as so<br><br>&lt;style type=&quot;text/css&quot;&gt;<br>.logo {<br>&#xA0; &#xA0; background: url(&quot;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">base64_encode_image </span><span class="keyword">(</span><span class="string">&apos;img/logo.png&apos;</span><span class="keyword">,</span><span class="string">&apos;png&apos;</span><span class="keyword">); </span><span class="default">?&gt;</span>&quot;) no-repeat right 5px;<br>}<br>&lt;/style&gt;<br><br>or<br><br>&lt;img src=&quot;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">base64_encode_image </span><span class="keyword">(</span><span class="string">&apos;img/logo.png&apos;</span><span class="keyword">,</span><span class="string">&apos;png&apos;</span><span class="keyword">); </span><span class="default">?&gt;</span>&quot;/&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
Unfortunately my &quot;function&quot; for encoding base64 on-the-fly from 2007 [which has been removed from the manual in favor of this post] had 2 errors!
<br>The first led to an endless loop because of a missing &quot;$feof&quot;-check, the second caused the rare mentioned errors when encoding failed for some reason in larger files, especially when
<br>setting fgets($fh, 2) for example. But lower values then 1024 are bad overall because they slow down the whole process, so 4096 will be fine for all purposes, I guess.
<br>The error was caused by the use of &quot;empty()&quot;.
<br>
<br>Here comes the corrected version which I have tested for all kind of files and length (up to 4,5 Gb!) without any error:
<br>
<br><span class="default">&lt;?php
<br>$fh </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;Input-File&apos;</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">);
<br></span><span class="comment">//$fh2 = fopen(&apos;Output-File&apos;, &apos;wb&apos;);
<br>
<br></span><span class="default">$cache </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br></span><span class="default">$eof </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>
<br>while (</span><span class="default">1</span><span class="keyword">) {
<br>
<br>&#xA0; &#xA0; if (!</span><span class="default">$eof</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row </span><span class="keyword">= </span><span class="default">fgets</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, </span><span class="default">4096</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$eof </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$cache </span><span class="keyword">!== </span><span class="string">&apos;&apos;</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row </span><span class="keyword">= </span><span class="default">$cache</span><span class="keyword">.</span><span class="default">$row</span><span class="keyword">;
<br>&#xA0; &#xA0; elseif (</span><span class="default">$eof</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>
<br>&#xA0; &#xA0; </span><span class="default">$b64 </span><span class="keyword">= </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$put </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$b64</span><span class="keyword">) &lt; </span><span class="default">76</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$eof</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$put </span><span class="keyword">= </span><span class="default">$b64</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cache </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cache </span><span class="keyword">= </span><span class="default">$row</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; } elseif (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$b64</span><span class="keyword">) &gt; </span><span class="default">76</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; do {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$put </span><span class="keyword">.= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$b64</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">76</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$b64 </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$b64</span><span class="keyword">, </span><span class="default">76</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; } while (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$b64</span><span class="keyword">) &gt; </span><span class="default">76</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cache </span><span class="keyword">= </span><span class="default">base64_decode</span><span class="keyword">(</span><span class="default">$b64</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$eof </span><span class="keyword">&amp;&amp; </span><span class="default">$b64</span><span class="keyword">{</span><span class="default">75</span><span class="keyword">} == </span><span class="string">&apos;=&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cache </span><span class="keyword">= </span><span class="default">$row</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$put </span><span class="keyword">= </span><span class="default">$b64</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cache </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$put </span><span class="keyword">!== </span><span class="string">&apos;&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$put</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//fputs($fh2, $put);
<br>&#xA0; &#xA0; &#xA0; &#xA0; //fputs($fh2, base64_decode($put));&#xA0; &#xA0; &#xA0; &#xA0; // for comparing
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>}
<br>
<br></span><span class="comment">//fclose($fh2);
<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
function urlsafe_b64encode($string) {<br>&#xA0; &#xA0; $data = base64_encode($string);<br>&#xA0; &#xA0; $data = str_replace(array(&apos;+&apos;,&apos;/&apos;,&apos;=&apos;),array(&apos;-&apos;,&apos;_&apos;,&apos;&apos;),$data);<br>&#xA0; &#xA0; return $data;<br>}<br><br>function urlsafe_b64decode($string) {<br>&#xA0; &#xA0; $data = str_replace(array(&apos;-&apos;,&apos;_&apos;),array(&apos;+&apos;,&apos;/&apos;),$string);<br>&#xA0; &#xA0; $mod4 = strlen($data) % 4;<br>&#xA0; &#xA0; if ($mod4) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $data .= substr(&apos;====&apos;, $mod4);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return base64_decode($data);<br>}<br><br>Php version of perl&apos;s MIME::Base64::URLSafe, that provides an url-safe base64 string encoding/decoding (compatible with python base64&apos;s urlsafe methods)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.base64-encode.php)

**[⬆ to root](/)**