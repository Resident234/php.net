# $http_response_header




<div class="phpcode"><span class="html">
Note that the HTTP wrapper has a hard limit of 1024 characters for the header lines.<br>Any HTTP header received that is longer than this will be ignored and won&apos;t appear in $http_response_header.<br><br>The cURL extension doesn&apos;t have this limit.<br><br>http_fopen_wrapper.c: #define HTTP_HEADER_BLOCK_SIZE 1024</span>
</div>
  

#


<div class="phpcode"><span class="html">
parser function to get formatted headers (with response code)<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">parseHeaders</span><span class="keyword">( </span><span class="default">$headers </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$head </span><span class="keyword">= array();<br>&#xA0; &#xA0; foreach( </span><span class="default">$headers </span><span class="keyword">as </span><span class="default">$k</span><span class="keyword">=&gt;</span><span class="default">$v </span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$t </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">( </span><span class="string">&apos;:&apos;</span><span class="keyword">, </span><span class="default">$v</span><span class="keyword">, </span><span class="default">2 </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if( isset( </span><span class="default">$t</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] ) )<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$head</span><span class="keyword">[ </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$t</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) ] = </span><span class="default">trim</span><span class="keyword">( </span><span class="default">$t</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] );<br>&#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$head</span><span class="keyword">[] = </span><span class="default">$v</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if( </span><span class="default">preg_match</span><span class="keyword">( </span><span class="string">&quot;#HTTP/[0-9\.]+\s+([0-9]+)#&quot;</span><span class="keyword">,</span><span class="default">$v</span><span class="keyword">, </span><span class="default">$out </span><span class="keyword">) )<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$head</span><span class="keyword">[</span><span class="string">&apos;reponse_code&apos;</span><span class="keyword">] = </span><span class="default">intval</span><span class="keyword">(</span><span class="default">$out</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$head</span><span class="keyword">;<br>}<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">parseHeaders</span><span class="keyword">(</span><span class="default">$http_response_header</span><span class="keyword">));<br><br></span><span class="comment">/*<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; HTTP/1.1 200 OK<br>&#xA0; &#xA0; [reponse_code] =&gt; 200<br>&#xA0; &#xA0; [Date] =&gt; Fri, 01 May 2015 12:56:09 GMT<br>&#xA0; &#xA0; [Server] =&gt; Apache<br>&#xA0; &#xA0; [X-Powered-By] =&gt; PHP/5.3.3-7+squeeze18<br>&#xA0; &#xA0; [Set-Cookie] =&gt; PHPSESSID=ng25jekmlipl1smfscq7copdl3; path=/<br>&#xA0; &#xA0; [Expires] =&gt; Thu, 19 Nov 1981 08:52:00 GMT<br>&#xA0; &#xA0; [Cache-Control] =&gt; no-store, no-cache, must-revalidate, post-check=0, pre-check=0<br>&#xA0; &#xA0; [Pragma] =&gt; no-cache<br>&#xA0; &#xA0; [Vary] =&gt; Accept-Encoding<br>&#xA0; &#xA0; [Content-Length] =&gt; 872<br>&#xA0; &#xA0; [Connection] =&gt; close<br>&#xA0; &#xA0; [Content-Type] =&gt; text/html<br>)<br>*/<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.httpresponseheader.php)

**[To root](/)**