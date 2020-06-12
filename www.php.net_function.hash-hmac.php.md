# hash_hmac




<div class="phpcode"><span class="html">
Very important notice, if you pass array to $data, php will generate a Warning, return a NULL and continue your application. Which I think is critical vulnerability as this function used to check authorisation typically.<br><br>Example:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">hash_hmac</span><span class="keyword">(</span><span class="string">&apos;sha256&apos;</span><span class="keyword">, [], </span><span class="string">&apos;secret&apos;</span><span class="keyword">));<br><br></span><span class="default">WARNING hash_hmac</span><span class="keyword">() </span><span class="default">expects parameter 2 to be string</span><span class="keyword">, array </span><span class="default">given on line number 3<br>NULL<br>?&gt;<br></span>Of course not documented feature.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please be careful when comparing hashes. In certain cases, information can be leaked by using a timing attack. It takes advantage of the == operator only comparing until it finds a difference in the two strings. To prevent it, you have two options.
<br>
<br>Option 1: hash both hashed strings first - this doesn&apos;t stop the timing difference, but it makes the information useless.
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">md5</span><span class="keyword">(</span><span class="default">$hashed_value</span><span class="keyword">) === </span><span class="default">md5</span><span class="keyword">(</span><span class="default">$hashed_expected</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;hashes match!&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;
<br></span>
<br>Option 2: always compare the whole string.
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">hash_compare</span><span class="keyword">(</span><span class="default">$hashed_value</span><span class="keyword">, </span><span class="default">$hashed_expected</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;hashes match!&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">hash_compare</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">) || !</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$len </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$len </span><span class="keyword">!== </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$len</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">|= </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]) ^ </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$status </span><span class="keyword">=== </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As&#xA0; Michael&#xA0; uggests we should take care not to compare the hash using == (or ===). Since PHP version 5.6 we can now use hash_equals().<br><br>So the example will be:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">hash_equals</span><span class="keyword">(</span><span class="default">$hashed_expected</span><span class="keyword">, </span><span class="default">$hashed_value</span><span class="keyword">) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;hashes match!&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For signing an Amazon AWS query, base64-encode the binary value:<br><br><span class="default">&lt;?php<br>&#xA0; $Sig </span><span class="keyword">= </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">hash_hmac</span><span class="keyword">(</span><span class="string">&apos;sha256&apos;</span><span class="keyword">, </span><span class="default">$Request</span><span class="keyword">, </span><span class="default">$AmazonSecretKey</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-hmac.php)

**[⬆ to root](/)**