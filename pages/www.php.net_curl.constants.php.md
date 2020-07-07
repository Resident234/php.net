# Predefined Constants




<div class="phpcode"><span class="html">
I hope this would be useful to convert error codes:<br><br><span class="default">&lt;?php<br>$curl_errno </span><span class="keyword">= array(<br></span><span class="default">1&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_UNSUPPORTED_PROTOCOL&quot;</span><span class="keyword">,<br></span><span class="default">2&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FAILED_INIT&quot;</span><span class="keyword">,<br></span><span class="default">3&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_URL_MALFORMAT&quot;</span><span class="keyword">,<br></span><span class="default">4&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_URL_MALFORMAT_USER&quot;</span><span class="keyword">,<br></span><span class="default">5&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_COULDNT_RESOLVE_PROXY&quot;</span><span class="keyword">,<br></span><span class="default">6&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_COULDNT_RESOLVE_HOST&quot;</span><span class="keyword">,<br></span><span class="default">7&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_COULDNT_CONNECT&quot;</span><span class="keyword">,<br></span><span class="default">8&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_WEIRD_SERVER_REPLY&quot;</span><span class="keyword">,<br></span><span class="default">9&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_ACCESS_DENIED&quot;</span><span class="keyword">,<br></span><span class="default">10 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_USER_PASSWORD_INCORRECT&quot;</span><span class="keyword">,<br></span><span class="default">11 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_WEIRD_PASS_REPLY&quot;</span><span class="keyword">,<br></span><span class="default">12 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_WEIRD_USER_REPLY&quot;</span><span class="keyword">,<br></span><span class="default">13 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_WEIRD_PASV_REPLY&quot;</span><span class="keyword">,<br></span><span class="default">14 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_WEIRD_227_FORMAT&quot;</span><span class="keyword">,<br></span><span class="default">15 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_CANT_GET_HOST&quot;</span><span class="keyword">,<br></span><span class="default">16 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_CANT_RECONNECT&quot;</span><span class="keyword">,<br></span><span class="default">17 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_COULDNT_SET_BINARY&quot;</span><span class="keyword">,<br></span><span class="default">18 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_PARTIAL_FILE or CURLE_PARTIAL_FILE&quot;</span><span class="keyword">,<br></span><span class="default">19 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_COULDNT_RETR_FILE&quot;</span><span class="keyword">,<br></span><span class="default">20 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_WRITE_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">21 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_QUOTE_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">22 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_HTTP_NOT_FOUND or CURLE_HTTP_RETURNED_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">23 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_WRITE_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">24 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_MALFORMAT_USER&quot;</span><span class="keyword">,<br></span><span class="default">25 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_COULDNT_STOR_FILE&quot;</span><span class="keyword">,<br></span><span class="default">26 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_READ_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">27 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_OUT_OF_MEMORY&quot;</span><span class="keyword">,<br></span><span class="default">28 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_OPERATION_TIMEDOUT or CURLE_OPERATION_TIMEOUTED&quot;</span><span class="keyword">,<br></span><span class="default">29 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_COULDNT_SET_ASCII&quot;</span><span class="keyword">,<br></span><span class="default">30 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_PORT_FAILED&quot;</span><span class="keyword">,<br></span><span class="default">31 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_COULDNT_USE_REST&quot;</span><span class="keyword">,<br></span><span class="default">32 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_COULDNT_GET_SIZE&quot;</span><span class="keyword">,<br></span><span class="default">33 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_HTTP_RANGE_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">34 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_HTTP_POST_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">35 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_CONNECT_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">36 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_BAD_DOWNLOAD_RESUME or CURLE_FTP_BAD_DOWNLOAD_RESUME&quot;</span><span class="keyword">,<br></span><span class="default">37 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FILE_COULDNT_READ_FILE&quot;</span><span class="keyword">,<br></span><span class="default">38 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_LDAP_CANNOT_BIND&quot;</span><span class="keyword">,<br></span><span class="default">39 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_LDAP_SEARCH_FAILED&quot;</span><span class="keyword">,<br></span><span class="default">40 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_LIBRARY_NOT_FOUND&quot;</span><span class="keyword">,<br></span><span class="default">41 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FUNCTION_NOT_FOUND&quot;</span><span class="keyword">,<br></span><span class="default">42 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_ABORTED_BY_CALLBACK&quot;</span><span class="keyword">,<br></span><span class="default">43 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_BAD_FUNCTION_ARGUMENT&quot;</span><span class="keyword">,<br></span><span class="default">44 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_BAD_CALLING_ORDER&quot;</span><span class="keyword">,<br></span><span class="default">45 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_HTTP_PORT_FAILED&quot;</span><span class="keyword">,<br></span><span class="default">46 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_BAD_PASSWORD_ENTERED&quot;</span><span class="keyword">,<br></span><span class="default">47 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_TOO_MANY_REDIRECTS&quot;</span><span class="keyword">,<br></span><span class="default">48 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_UNKNOWN_TELNET_OPTION&quot;</span><span class="keyword">,<br></span><span class="default">49 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_TELNET_OPTION_SYNTAX&quot;</span><span class="keyword">,<br></span><span class="default">50 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_OBSOLETE&quot;</span><span class="keyword">,<br></span><span class="default">51 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_PEER_CERTIFICATE&quot;</span><span class="keyword">,<br></span><span class="default">52 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_GOT_NOTHING&quot;</span><span class="keyword">,<br></span><span class="default">53 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_ENGINE_NOTFOUND&quot;</span><span class="keyword">,<br></span><span class="default">54 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_ENGINE_SETFAILED&quot;</span><span class="keyword">,<br></span><span class="default">55 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SEND_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">56 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_RECV_ERROR&quot;</span><span class="keyword">,<br></span><span class="default">57 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SHARE_IN_USE&quot;</span><span class="keyword">,<br></span><span class="default">58 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_CERTPROBLEM&quot;</span><span class="keyword">,<br></span><span class="default">59 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_CIPHER&quot;</span><span class="keyword">,<br></span><span class="default">60 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSL_CACERT&quot;</span><span class="keyword">,<br></span><span class="default">61 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_BAD_CONTENT_ENCODING&quot;</span><span class="keyword">,<br></span><span class="default">62 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_LDAP_INVALID_URL&quot;</span><span class="keyword">,<br></span><span class="default">63 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FILESIZE_EXCEEDED&quot;</span><span class="keyword">,<br></span><span class="default">64 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_FTP_SSL_FAILED&quot;</span><span class="keyword">,<br></span><span class="default">79 </span><span class="keyword">=&gt; </span><span class="string">&quot;CURLE_SSH&quot;<br></span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware of CURLE_* constants!<br><br>On the official site:<br><br><a href="http://curl.haxx.se/libcurl/c/libcurl-errors.html" rel="nofollow" target="_blank">http://curl.haxx.se/libcurl/c/libcurl-errors.html</a><br><br>some constants are different, some missing compared to the PHP implementation.<br><br>Some examples:<br><br>in PHP the curl error number 28 is called<br><br>CURLE_OPERATION_TIMEOUTED<br><br>while in the official site is:<br><br>CURLE_OPERATION_TIMEDOUT<br><br>So if you use the second, it won&apos;t march the error 28 because in PHP it is not defined that way.<br><br>The same is for these:<br><br>CURLE_HTTP_RETURNED_ERROR<br>CURLE_UPLOAD_FAILED<br>CURLE_INTERFACE_FAILED<br>CURLE_SSL_CERTPROBLEM<br>CURLE_SEND_FAIL_REWIND<br>CURLE_LOGIN_DENIED<br>CURLE_AGAIN<br><br>that are in someway named differently or missing from PHP.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/curl.constants.php)

**[To root](/README.md)**