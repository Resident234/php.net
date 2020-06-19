# data://




<div class="phpcode"><span class="html">
When passing plain string without base64 encoding, do not forget to pass the string through URLENCODE(), because PHP automatically urldecodes all entities inside passed string (and therefore all + get lost, all % entities will be converted to the corresponding characters).
<br>
<br>In this case, PHP is strictly compilant with the RFC 2397. Section 3 states that passes data should be either in base64 encoding or urlencoded.
<br>
<br>VALID USAGE:
<br><span class="default">&lt;?php
<br>$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;data:text/plain,&apos;</span><span class="keyword">.</span><span class="default">urlencode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">), </span><span class="string">&apos;rb&apos;</span><span class="keyword">); </span><span class="comment">// urlencoded data
<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;data:text/plain;base64,&apos;</span><span class="keyword">.</span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">), </span><span class="string">&apos;rb&apos;</span><span class="keyword">); </span><span class="comment">// base64 encoded data
<br></span><span class="default">?&gt;
<br></span>
<br>Demonstration of invalid usage:
<br><span class="default">&lt;?php
<br>$data </span><span class="keyword">= </span><span class="string">&apos;G&#xFC;nther says: 1+1 is 2, 10%40 is 20.&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;data:text/plain,&apos;</span><span class="keyword">.</span><span class="default">$data</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">); </span><span class="comment">// INVALID, never do this
<br></span><span class="keyword">echo </span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br></span><span class="comment">// G&#xFC;nther says: 1 1 is 2, 10@ is 20. // ERROR
<br>
<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;data:text/plain,&apos;</span><span class="keyword">.</span><span class="default">urlencode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">), </span><span class="string">&apos;rb&apos;</span><span class="keyword">); </span><span class="comment">// urlencoded data
<br></span><span class="keyword">echo </span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br></span><span class="comment">// G&#xFC;nther says: 1+1 is 2, 10%40 is 20. // OK
<br>
<br>// Valid option 1: base64 encoded data
<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;data:text/plain;base64,&apos;</span><span class="keyword">.</span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">), </span><span class="string">&apos;rb&apos;</span><span class="keyword">); </span><span class="comment">// base64 encoded data
<br></span><span class="keyword">echo </span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br></span><span class="comment">// G&#xFC;nther says: 1+1 is 2, 10%40 is 20. // OK
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to create a gd-image directly out of a sql-database-field you might want to use:
<br>
<br><span class="default">&lt;?php
<br>$jpegimage </span><span class="keyword">= </span><span class="default">imagecreatefromjpeg</span><span class="keyword">(</span><span class="string">&quot;data://image/jpeg;base64,&quot; </span><span class="keyword">. </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$sql_result_array</span><span class="keyword">[</span><span class="string">&apos;imagedata&apos;</span><span class="keyword">]));
<br></span><span class="default">?&gt;
<br></span>
<br>this goes also for gif, png, etc using the correct &quot;imagecreatefrom$$$&quot;-function and mime-type.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.data.php)

**[To root](/README.md)**