# curl_error



If you want to fetch the error message, make sure you fetch it before you close the current cURL session or the error message will be reset to an empty string.  

---

For a 404 response to actually trigger an error as the example seems to be trying to demonstrate the following option should be set:<br><br>curl_setopt($ch,CURLOPT_FAILONERROR,true);<br><br>As per http://curl.haxx.se/libcurl/c/libcurl-errors.html<br><br>CURLE_HTTP_RETURNED_ERROR (22)<br>This is returned if CURLOPT_FAILONERROR is set TRUE and the HTTP server returns an error code that is &gt;= 400. (This error code was formerly known as CURLE_HTTP_NOT_FOUND.)  

---

[Official documentation page](https://www.php.net/manual/en/function.curl-error.php)

**[To root](/README.md)**