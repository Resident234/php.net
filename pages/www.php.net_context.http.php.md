# HTTP context options





Note that if you set the protocol_version option to 1.1 and the server you are requesting from is configured to use keep-alive connections, the function (fopen, file_get_contents, etc.) will &quot;be slow&quot; and take a long time to complete. This is a feature of the HTTP 1.1 protocol you are unlikely to use with stream contexts in PHP.

Simply add a &quot;Connection: close&quot; header to the request to eliminate the keep-alive timeout:



```
<?php
// php 5.4 : array syntax and header option with array value
$data = file_get_contents(&apos;http://www.example.com/&apos;, null, stream_context_create([
&#xA0; &#xA0; &apos;http&apos; =&gt; [
&#xA0; &#xA0; &#xA0; &#xA0; &apos;protocol_version&apos; =&gt; 1.1,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;header&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; =&gt; [
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;Connection: close&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; ],
&#xA0; &#xA0; ],
]));
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/context.http.php)

**[To root](/README.md)**