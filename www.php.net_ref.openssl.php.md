# OpenSSL Functions




<div class="phpcode"><span class="html">
Currently, all OpenSSL Functions defined in PHP only utilize the PEM format.&#xA0; Use the following code to convert from DER to PEM and PEM to DER.<br><br><span class="default">&lt;?php<br>$pem_data </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$cert_path</span><span class="keyword">.</span><span class="default">$pem_file</span><span class="keyword">);<br></span><span class="default">$pem2der </span><span class="keyword">= </span><span class="default">pem2der</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">);<br><br></span><span class="default">$der_data </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$cert_path</span><span class="keyword">.</span><span class="default">$der_file</span><span class="keyword">);<br></span><span class="default">$der2pem </span><span class="keyword">= </span><span class="default">der2pem</span><span class="keyword">(</span><span class="default">$der_data</span><span class="keyword">);<br><br>function </span><span class="default">pem2der</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">) {<br>&#xA0;&#xA0; </span><span class="default">$begin </span><span class="keyword">= </span><span class="string">&quot;CERTIFICATE-----&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; </span><span class="default">$end&#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&quot;-----END&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; </span><span class="default">$pem_data </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">, </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">, </span><span class="default">$begin</span><span class="keyword">)+</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$begin</span><span class="keyword">));&#xA0; &#xA0; <br>&#xA0;&#xA0; </span><span class="default">$pem_data </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">, </span><span class="default">$end</span><span class="keyword">));<br>&#xA0;&#xA0; </span><span class="default">$der </span><span class="keyword">= </span><span class="default">base64_decode</span><span class="keyword">(</span><span class="default">$pem_data</span><span class="keyword">);<br>&#xA0;&#xA0; return </span><span class="default">$der</span><span class="keyword">;<br>}<br><br>function </span><span class="default">der2pem</span><span class="keyword">(</span><span class="default">$der_data</span><span class="keyword">) {<br>&#xA0;&#xA0; </span><span class="default">$pem </span><span class="keyword">= </span><span class="default">chunk_split</span><span class="keyword">(</span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$der_data</span><span class="keyword">), </span><span class="default">64</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>&#xA0;&#xA0; </span><span class="default">$pem </span><span class="keyword">= </span><span class="string">&quot;-----BEGIN CERTIFICATE-----\n&quot;</span><span class="keyword">.</span><span class="default">$pem</span><span class="keyword">.</span><span class="string">&quot;-----END CERTIFICATE-----\n&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; return </span><span class="default">$pem</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.openssl.php)

**[To root](/README.md)**