# $http_response_header



Note that the HTTP wrapper has a hard limit of 1024 characters for the header lines.<br>Any HTTP header received that is longer than this will be ignored and won&apos;t appear in $http_response_header.<br><br>The cURL extension doesn&apos;t have this limit.<br><br>http_fopen_wrapper.c: #define HTTP_HEADER_BLOCK_SIZE 1024  

---

parser function to get formatted headers (with response code)<br><br>

```
<?php

function parseHeaders( $headers )
{
    $head = array();
    foreach( $headers as $k=>$v )
    {
        $t = explode( ':', $v, 2 );
        if( isset( $t[1] ) )
            $head[ trim($t[0]) ] = trim( $t[1] );
        else
        {
            $head[] = $v;
            if( preg_match( "#HTTP/[0-9\.]+\s+([0-9]+)#",$v, $out ) )
                $head['reponse_code'] = intval($out[1]);
        }
    }
    return $head;
}

print_r(parseHeaders($http_response_header));

/*
Array
(
    [0] => HTTP/1.1 200 OK
    [reponse_code] => 200
    [Date] => Fri, 01 May 2015 12:56:09 GMT
    [Server] => Apache
    [X-Powered-By] => PHP/5.3.3-7+squeeze18
    [Set-Cookie] => PHPSESSID=ng25jekmlipl1smfscq7copdl3; path=/
    [Expires] => Thu, 19 Nov 1981 08:52:00 GMT
    [Cache-Control] => no-store, no-cache, must-revalidate, post-check=0, pre-check=0
    [Pragma] => no-cache
    [Vary] => Accept-Encoding
    [Content-Length] => 872
    [Connection] => close
    [Content-Type] => text/html
)
*/

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.httpresponseheader.php)

**[To root](/README.md)**