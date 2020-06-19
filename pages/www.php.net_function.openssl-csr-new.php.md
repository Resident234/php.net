# openssl_csr_new




<div class="phpcode"><span class="html">
Not sure whether the &quot;bug&quot; (undocumented behavior) I encountered is common to other people, but this comment might save hours of painful debug:<br>If you can&apos;t generate a new private key using openssl_pkey_new() or openssl_csr_new(), your script hangs during the call of these functions and in case you specified a &quot;private_key_bits&quot; parameter, ensure that you cast the variable to an int. Took me ages to notice that.<br><br><span class="default">&lt;?php<br>$SSLcnf </span><span class="keyword">= array(</span><span class="string">&apos;config&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;/usr/local/nessy2/share/ssl/openssl.cnf&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;encrypt_key&apos; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;private_key_type&apos; </span><span class="keyword">=&gt; </span><span class="default">OPENSSL_KEYTYPE_RSA</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;digest_alg&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;sha1&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;x509_extensions&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;v3_ca&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;private_key_bits&apos; </span><span class="keyword">=&gt; </span><span class="default">$someVariable </span><span class="comment">// ---&gt; bad<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;private_key_bits&apos; </span><span class="keyword">=&gt; (int)</span><span class="default">$someVariable </span><span class="comment">// ---&gt; good<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;private_key_bits&apos; </span><span class="keyword">=&gt; </span><span class="default">512 </span><span class="comment">// ---&gt; obviously good<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-csr-new.php)

**[To root](/README.md)**