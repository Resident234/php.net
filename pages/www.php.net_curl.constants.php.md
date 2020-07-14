# Predefined Constants



I hope this would be useful to convert error codes:<br><br>

```
<?php
$curl_errno = array(
1  =&gt; "CURLE_UNSUPPORTED_PROTOCOL",
2  =&gt; "CURLE_FAILED_INIT",
3  =&gt; "CURLE_URL_MALFORMAT",
4  =&gt; "CURLE_URL_MALFORMAT_USER",
5  =&gt; "CURLE_COULDNT_RESOLVE_PROXY",
6  =&gt; "CURLE_COULDNT_RESOLVE_HOST",
7  =&gt; "CURLE_COULDNT_CONNECT",
8  =&gt; "CURLE_FTP_WEIRD_SERVER_REPLY",
9  =&gt; "CURLE_FTP_ACCESS_DENIED",
10 =&gt; "CURLE_FTP_USER_PASSWORD_INCORRECT",
11 =&gt; "CURLE_FTP_WEIRD_PASS_REPLY",
12 =&gt; "CURLE_FTP_WEIRD_USER_REPLY",
13 =&gt; "CURLE_FTP_WEIRD_PASV_REPLY",
14 =&gt; "CURLE_FTP_WEIRD_227_FORMAT",
15 =&gt; "CURLE_FTP_CANT_GET_HOST",
16 =&gt; "CURLE_FTP_CANT_RECONNECT",
17 =&gt; "CURLE_FTP_COULDNT_SET_BINARY",
18 =&gt; "CURLE_FTP_PARTIAL_FILE or CURLE_PARTIAL_FILE",
19 =&gt; "CURLE_FTP_COULDNT_RETR_FILE",
20 =&gt; "CURLE_FTP_WRITE_ERROR",
21 =&gt; "CURLE_FTP_QUOTE_ERROR",
22 =&gt; "CURLE_HTTP_NOT_FOUND or CURLE_HTTP_RETURNED_ERROR",
23 =&gt; "CURLE_WRITE_ERROR",
24 =&gt; "CURLE_MALFORMAT_USER",
25 =&gt; "CURLE_FTP_COULDNT_STOR_FILE",
26 =&gt; "CURLE_READ_ERROR",
27 =&gt; "CURLE_OUT_OF_MEMORY",
28 =&gt; "CURLE_OPERATION_TIMEDOUT or CURLE_OPERATION_TIMEOUTED",
29 =&gt; "CURLE_FTP_COULDNT_SET_ASCII",
30 =&gt; "CURLE_FTP_PORT_FAILED",
31 =&gt; "CURLE_FTP_COULDNT_USE_REST",
32 =&gt; "CURLE_FTP_COULDNT_GET_SIZE",
33 =&gt; "CURLE_HTTP_RANGE_ERROR",
34 =&gt; "CURLE_HTTP_POST_ERROR",
35 =&gt; "CURLE_SSL_CONNECT_ERROR",
36 =&gt; "CURLE_BAD_DOWNLOAD_RESUME or CURLE_FTP_BAD_DOWNLOAD_RESUME",
37 =&gt; "CURLE_FILE_COULDNT_READ_FILE",
38 =&gt; "CURLE_LDAP_CANNOT_BIND",
39 =&gt; "CURLE_LDAP_SEARCH_FAILED",
40 =&gt; "CURLE_LIBRARY_NOT_FOUND",
41 =&gt; "CURLE_FUNCTION_NOT_FOUND",
42 =&gt; "CURLE_ABORTED_BY_CALLBACK",
43 =&gt; "CURLE_BAD_FUNCTION_ARGUMENT",
44 =&gt; "CURLE_BAD_CALLING_ORDER",
45 =&gt; "CURLE_HTTP_PORT_FAILED",
46 =&gt; "CURLE_BAD_PASSWORD_ENTERED",
47 =&gt; "CURLE_TOO_MANY_REDIRECTS",
48 =&gt; "CURLE_UNKNOWN_TELNET_OPTION",
49 =&gt; "CURLE_TELNET_OPTION_SYNTAX",
50 =&gt; "CURLE_OBSOLETE",
51 =&gt; "CURLE_SSL_PEER_CERTIFICATE",
52 =&gt; "CURLE_GOT_NOTHING",
53 =&gt; "CURLE_SSL_ENGINE_NOTFOUND",
54 =&gt; "CURLE_SSL_ENGINE_SETFAILED",
55 =&gt; "CURLE_SEND_ERROR",
56 =&gt; "CURLE_RECV_ERROR",
57 =&gt; "CURLE_SHARE_IN_USE",
58 =&gt; "CURLE_SSL_CERTPROBLEM",
59 =&gt; "CURLE_SSL_CIPHER",
60 =&gt; "CURLE_SSL_CACERT",
61 =&gt; "CURLE_BAD_CONTENT_ENCODING",
62 =&gt; "CURLE_LDAP_INVALID_URL",
63 =&gt; "CURLE_FILESIZE_EXCEEDED",
64 =&gt; "CURLE_FTP_SSL_FAILED",
79 =&gt; "CURLE_SSH"
);
?>
```
  

#

Beware of CURLE_* constants!<br><br>On the official site:<br><br>http://curl.haxx.se/libcurl/c/libcurl-errors.html<br><br>some constants are different, some missing compared to the PHP implementation.<br><br>Some examples:<br><br>in PHP the curl error number 28 is called<br><br>CURLE_OPERATION_TIMEOUTED<br><br>while in the official site is:<br><br>CURLE_OPERATION_TIMEDOUT<br><br>So if you use the second, it won&apos;t march the error 28 because in PHP it is not defined that way.<br><br>The same is for these:<br><br>CURLE_HTTP_RETURNED_ERROR<br>CURLE_UPLOAD_FAILED<br>CURLE_INTERFACE_FAILED<br>CURLE_SSL_CERTPROBLEM<br>CURLE_SEND_FAIL_REWIND<br>CURLE_LOGIN_DENIED<br>CURLE_AGAIN<br><br>that are in someway named differently or missing from PHP.  

#

[Official documentation page](https://www.php.net/manual/en/curl.constants.php)

**[To root](/README.md)**