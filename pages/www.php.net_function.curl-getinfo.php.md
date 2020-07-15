# curl_getinfo



Here are the response codes ready for pasting in an ini-style file. Can be used to provide more descriptive message, corresponding to &apos;http_code&apos; index of the arrray returned by curl_getinfo(). <br>These are taken from the W3 consortium HTTP/1.1: Status Code Definitions, found at<br>http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html<br><br>[Informational 1xx]<br>100="Continue"<br>101="Switching Protocols"<br><br>[Successful 2xx]<br>200="OK"<br>201="Created"<br>202="Accepted"<br>203="Non-Authoritative Information"<br>204="No Content"<br>205="Reset Content"<br>206="Partial Content"<br><br>[Redirection 3xx]<br>300="Multiple Choices"<br>301="Moved Permanently"<br>302="Found"<br>303="See Other"<br>304="Not Modified"<br>305="Use Proxy"<br>306="(Unused)"<br>307="Temporary Redirect"<br><br>[Client Error 4xx]<br>400="Bad Request"<br>401="Unauthorized"<br>402="Payment Required"<br>403="Forbidden"<br>404="Not Found"<br>405="Method Not Allowed"<br>406="Not Acceptable"<br>407="Proxy Authentication Required"<br>408="Request Timeout"<br>409="Conflict"<br>410="Gone"<br>411="Length Required"<br>412="Precondition Failed"<br>413="Request Entity Too Large"<br>414="Request-URI Too Long"<br>415="Unsupported Media Type"<br>416="Requested Range Not Satisfiable"<br>417="Expectation Failed"<br><br>[Server Error 5xx]<br>500="Internal Server Error"<br>501="Not Implemented"<br>502="Bad Gateway"<br>503="Service Unavailable"<br>504="Gateway Timeout"<br>505="HTTP Version Not Supported"<br><br>And an example usage:<br>

```
<?php
$ch = curl_init(); // create cURL handle (ch)
if (!$ch) {
    die("Couldn't initialize a cURL handle");
}
// set some cURL options
$ret = curl_setopt($ch, CURLOPT_URL,            "http://mail.yahoo.com");
$ret = curl_setopt($ch, CURLOPT_HEADER,         1);
$ret = curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
$ret = curl_setopt($ch, CURLOPT_RETURNTRANSFER, 0);
$ret = curl_setopt($ch, CURLOPT_TIMEOUT,        30);

// execute
$ret = curl_exec($ch);

if (empty($ret)) {
    // some kind of an error happened
    die(curl_error($ch));
    curl_close($ch); // close cURL handler
} else {
    $info = curl_getinfo($ch);
    curl_close($ch); // close cURL handler

    if (empty($info['http_code'])) {
            die("No HTTP code was returned"); 
    } else {
        // load the HTTP codes
        $http_codes = parse_ini_file("path/to/the/ini/file/I/pasted/above");
        
        // echo results
        echo "The server responded: &lt;br /&gt;";
        echo $info['http_code'] . " " . $http_codes[$info['http_code']];
    }

}
?>
```
  

#

CURLINFO_SSL_VERIFYRESULT error codes:<br>0: ok the operation was successful. <br>2 : unable to get issuer certificate<br>3: unable to get certificate CRL<br>4: unable to decrypt certificate&apos;s signature<br>5: unable to decrypt CRL&apos;s signature<br>6: unable to decode issuer public key<br>7: certificate signature failure<br>8: CRL signature failure<br>9: certificate is not yet valid<br>10: certificate has expired<br>11: CRL is not yet valid<br>12:CRL has expired<br>13: format error in certificate&apos;s notBefore field<br>14: format error in certificate&apos;s notAfter field<br>15: format error in CRL&apos;s lastUpdate field<br>16: format error in CRL&apos;s nextUpdate field<br>17: out of memory<br>18: self signed certificate<br>19: self signed certificate in certificate chain<br>20: unable to get local issuer certificate<br>21:unable to verify the first certificate<br>22: certificate chain too long<br>23: certificate revoked<br>24: invalid CA certificate<br>25: path length constraint exceeded<br>26: unsupported certificate purpose<br>27: certificate not trusted<br>28: certificate rejected<br>29: subject issuer mismatch<br>30: authority and subject key identifier mismatch<br>31: authority and issuer serial number mismatch<br>32: key usage does not include certificate signing<br>50: application verification failure<br>details at http://www.openssl.org/docs/apps/verify.html#VERIFY_OPERATION  

#

CURLINFO_HTTP_CODE does not return a string, as the docs say, but rather an integer.<br><br>

```
<?php
    $c = curl_init('http://www.example.com/');
    if(curl_getinfo($c, CURLINFO_HTTP_CODE) === '200') echo "CURLINFO_HTTP_CODE returns a string.";
    if(curl_getinfo($c, CURLINFO_HTTP_CODE) === 200) echo "CURLINFO_HTTP_CODE returns an integer.";
    curl_close($c);
?>
```
<br><br>returns<br><br>"CURLINFO_HTTP_CODE returns an integer."  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-getinfo.php)

**[To root](/README.md)**