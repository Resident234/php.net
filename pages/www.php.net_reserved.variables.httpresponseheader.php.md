# $http_response_header





Note that the HTTP wrapper has a hard limit of 1024 characters for the header lines.
Any HTTP header received that is longer than this will be ignored and won&apos;t appear in $http_response_header.

The cURL extension doesn&apos;t have this limit.

http_fopen_wrapper.c: #define HTTP_HEADER_BLOCK_SIZE 1024

  

#



parser function to get formatted headers (with response code)



```
<?php

function parseHeaders( $headers )
{
&#xA0; &#xA0; $head = array();
&#xA0; &#xA0; foreach( $headers as $k=&gt;$v )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $t = explode( &apos;:&apos;, $v, 2 );
&#xA0; &#xA0; &#xA0; &#xA0; if( isset( $t[1] ) )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $head[ trim($t[0]) ] = trim( $t[1] );
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $head[] = $v;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if( preg_match( &quot;#HTTP/[0-9\.]+\s+([0-9]+)#&quot;,$v, $out ) )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $head[&apos;reponse_code&apos;] = intval($out[1]);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; return $head;
}

print_r(parseHeaders($http_response_header));

/*
Array
(
&#xA0; &#xA0; [0] =&gt; HTTP/1.1 200 OK
&#xA0; &#xA0; [reponse_code] =&gt; 200
&#xA0; &#xA0; [Date] =&gt; Fri, 01 May 2015 12:56:09 GMT
&#xA0; &#xA0; [Server] =&gt; Apache
&#xA0; &#xA0; [X-Powered-By] =&gt; PHP/5.3.3-7+squeeze18
&#xA0; &#xA0; [Set-Cookie] =&gt; PHPSESSID=ng25jekmlipl1smfscq7copdl3; path=/
&#xA0; &#xA0; [Expires] =&gt; Thu, 19 Nov 1981 08:52:00 GMT
&#xA0; &#xA0; [Cache-Control] =&gt; no-store, no-cache, must-revalidate, post-check=0, pre-check=0
&#xA0; &#xA0; [Pragma] =&gt; no-cache
&#xA0; &#xA0; [Vary] =&gt; Accept-Encoding
&#xA0; &#xA0; [Content-Length] =&gt; 872
&#xA0; &#xA0; [Connection] =&gt; close
&#xA0; &#xA0; [Content-Type] =&gt; text/html
)
*/

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.httpresponseheader.php)

**[To root](/README.md)**