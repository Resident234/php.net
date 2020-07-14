# curl_errno



if someone need more information about curl errors<br>

```
<?php
$error_codes=array(
[1] =&gt; &apos;CURLE_UNSUPPORTED_PROTOCOL&apos;, 
[2] =&gt; &apos;CURLE_FAILED_INIT&apos;, 
[3] =&gt; &apos;CURLE_URL_MALFORMAT&apos;, 
[4] =&gt; &apos;CURLE_URL_MALFORMAT_USER&apos;, 
[5] =&gt; &apos;CURLE_COULDNT_RESOLVE_PROXY&apos;, 
[6] =&gt; &apos;CURLE_COULDNT_RESOLVE_HOST&apos;, 
[7] =&gt; &apos;CURLE_COULDNT_CONNECT&apos;, 
[8] =&gt; &apos;CURLE_FTP_WEIRD_SERVER_REPLY&apos;,
[9] =&gt; &apos;CURLE_REMOTE_ACCESS_DENIED&apos;,
[11] =&gt; &apos;CURLE_FTP_WEIRD_PASS_REPLY&apos;,
[13] =&gt; &apos;CURLE_FTP_WEIRD_PASV_REPLY&apos;,
[14]=&gt;&apos;CURLE_FTP_WEIRD_227_FORMAT&apos;,
[15] =&gt; &apos;CURLE_FTP_CANT_GET_HOST&apos;,
[17] =&gt; &apos;CURLE_FTP_COULDNT_SET_TYPE&apos;,
[18] =&gt; &apos;CURLE_PARTIAL_FILE&apos;,
[19] =&gt; &apos;CURLE_FTP_COULDNT_RETR_FILE&apos;,
[21] =&gt; &apos;CURLE_QUOTE_ERROR&apos;,
[22] =&gt; &apos;CURLE_HTTP_RETURNED_ERROR&apos;,
[23] =&gt; &apos;CURLE_WRITE_ERROR&apos;,
[25] =&gt; &apos;CURLE_UPLOAD_FAILED&apos;,
[26] =&gt; &apos;CURLE_READ_ERROR&apos;,
[27] =&gt; &apos;CURLE_OUT_OF_MEMORY&apos;,
[28] =&gt; &apos;CURLE_OPERATION_TIMEDOUT&apos;,
[30] =&gt; &apos;CURLE_FTP_PORT_FAILED&apos;,
[31] =&gt; &apos;CURLE_FTP_COULDNT_USE_REST&apos;,
[33] =&gt; &apos;CURLE_RANGE_ERROR&apos;,
[34] =&gt; &apos;CURLE_HTTP_POST_ERROR&apos;,
[35] =&gt; &apos;CURLE_SSL_CONNECT_ERROR&apos;,
[36] =&gt; &apos;CURLE_BAD_DOWNLOAD_RESUME&apos;,
[37] =&gt; &apos;CURLE_FILE_COULDNT_READ_FILE&apos;,
[38] =&gt; &apos;CURLE_LDAP_CANNOT_BIND&apos;,
[39] =&gt; &apos;CURLE_LDAP_SEARCH_FAILED&apos;,
[41] =&gt; &apos;CURLE_FUNCTION_NOT_FOUND&apos;,
[42] =&gt; &apos;CURLE_ABORTED_BY_CALLBACK&apos;,
[43] =&gt; &apos;CURLE_BAD_FUNCTION_ARGUMENT&apos;,
[45] =&gt; &apos;CURLE_INTERFACE_FAILED&apos;,
[47] =&gt; &apos;CURLE_TOO_MANY_REDIRECTS&apos;,
[48] =&gt; &apos;CURLE_UNKNOWN_TELNET_OPTION&apos;,
[49] =&gt; &apos;CURLE_TELNET_OPTION_SYNTAX&apos;,
[51] =&gt; &apos;CURLE_PEER_FAILED_VERIFICATION&apos;,
[52] =&gt; &apos;CURLE_GOT_NOTHING&apos;,
[53] =&gt; &apos;CURLE_SSL_ENGINE_NOTFOUND&apos;,
[54] =&gt; &apos;CURLE_SSL_ENGINE_SETFAILED&apos;,
[55] =&gt; &apos;CURLE_SEND_ERROR&apos;,
[56] =&gt; &apos;CURLE_RECV_ERROR&apos;,
[58] =&gt; &apos;CURLE_SSL_CERTPROBLEM&apos;,
[59] =&gt; &apos;CURLE_SSL_CIPHER&apos;,
[60] =&gt; &apos;CURLE_SSL_CACERT&apos;,
[61] =&gt; &apos;CURLE_BAD_CONTENT_ENCODING&apos;,
[62] =&gt; &apos;CURLE_LDAP_INVALID_URL&apos;,
[63] =&gt; &apos;CURLE_FILESIZE_EXCEEDED&apos;,
[64] =&gt; &apos;CURLE_USE_SSL_FAILED&apos;,
[65] =&gt; &apos;CURLE_SEND_FAIL_REWIND&apos;,
[66] =&gt; &apos;CURLE_SSL_ENGINE_INITFAILED&apos;,
[67] =&gt; &apos;CURLE_LOGIN_DENIED&apos;,
[68] =&gt; &apos;CURLE_TFTP_NOTFOUND&apos;,
[69] =&gt; &apos;CURLE_TFTP_PERM&apos;,
[70] =&gt; &apos;CURLE_REMOTE_DISK_FULL&apos;,
[71] =&gt; &apos;CURLE_TFTP_ILLEGAL&apos;,
[72] =&gt; &apos;CURLE_TFTP_UNKNOWNID&apos;,
[73] =&gt; &apos;CURLE_REMOTE_FILE_EXISTS&apos;,
[74] =&gt; &apos;CURLE_TFTP_NOSUCHUSER&apos;,
[75] =&gt; &apos;CURLE_CONV_FAILED&apos;,
[76] =&gt; &apos;CURLE_CONV_REQD&apos;,
[77] =&gt; &apos;CURLE_SSL_CACERT_BADFILE&apos;,
[78] =&gt; &apos;CURLE_REMOTE_FILE_NOT_FOUND&apos;,
[79] =&gt; &apos;CURLE_SSH&apos;,
[80] =&gt; &apos;CURLE_SSL_SHUTDOWN_FAILED&apos;,
[81] =&gt; &apos;CURLE_AGAIN&apos;,
[82] =&gt; &apos;CURLE_SSL_CRL_BADFILE&apos;,
[83] =&gt; &apos;CURLE_SSL_ISSUER_ERROR&apos;,
[84] =&gt; &apos;CURLE_FTP_PRET_FAILED&apos;,
[84] =&gt; &apos;CURLE_FTP_PRET_FAILED&apos;,
[85] =&gt; &apos;CURLE_RTSP_CSEQ_ERROR&apos;,
[86] =&gt; &apos;CURLE_RTSP_SESSION_ERROR&apos;,
[87] =&gt; &apos;CURLE_FTP_BAD_FILE_LIST&apos;,
[88] =&gt; &apos;CURLE_CHUNK_FAILED&apos;);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-errno.php)

**[To root](/README.md)**