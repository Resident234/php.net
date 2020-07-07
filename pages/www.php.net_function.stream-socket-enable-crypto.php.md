# stream_socket_enable_crypto




<div class="phpcode"><span class="html">
Constants added in PHP 5.6 :<br><br>STREAM_CRYPTO_METHOD_ANY_CLIENT<br>STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT<br>STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT<br>STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT<br>STREAM_CRYPTO_METHOD_ANY_SERVER<br>STREAM_CRYPTO_METHOD_TLSv1_0_SERVER<br>STREAM_CRYPTO_METHOD_TLSv1_1_SERVER<br>STREAM_CRYPTO_METHOD_TLSv1_2_SERVER<br><br>Now, be careful because since PHP 5.6.7, STREAM_CRYPTO_METHOD_TLS_CLIENT (same for _SERVER) no longer means any tls version but tls 1.0 only (for &quot;backward compatibility&quot;...).<br><br>Before PHP 5.6.7 :<br>STREAM_CRYPTO_METHOD_SSLv23_CLIENT = STREAM_CRYPTO_METHOD_SSLv2_CLIENT|STREAM_CRYPTO_METHOD_SSLv3_CLIENT<br>STREAM_CRYPTO_METHOD_TLS_CLIENT = STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT<br><br>PHP &gt;= 5.6.7<br>STREAM_CRYPTO_METHOD_SSLv23_CLIENT = STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT<br>STREAM_CRYPTO_METHOD_TLS_CLIENT = STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT<br><br>PHP bug : <a href="https://bugs.php.net/bug.php?id=69195" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=69195</a><br>Commit : <a href="https://github.com/php/php-src/commit/10bc5fd4c4c8e1dd57bd911b086e9872a56300a0" rel="nofollow" target="_blank">https://github.com/php/php-src/commit/10bc5fd4c4c8e1dd57bd911b086e9872a56300a0</a><br><br>STREAM_CRYPTO_METHOD_SSLv23_CLIENT is not safe to use because before php 5.6.7, it means sslv2 or sslv3. So, you should do this :<br><span class="default">&lt;?php<br>$crypto_method </span><span class="keyword">= </span><span class="default">STREAM_CRYPTO_METHOD_TLS_CLIENT</span><span class="keyword">;<br><br>if (</span><span class="default">defined</span><span class="keyword">(</span><span class="string">&apos;STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="default">$crypto_method </span><span class="keyword">|= </span><span class="default">STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$crypto_method </span><span class="keyword">|= </span><span class="default">STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT</span><span class="keyword">;<br>}<br><br></span><span class="default">stream_socket_enable_crypto</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">$crypto_method</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-socket-enable-crypto.php)

**[To root](/README.md)**