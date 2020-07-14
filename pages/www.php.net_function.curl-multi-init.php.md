# curl_multi_init



Here&apos;s an easier to follow example, From : http://arguments.callee.info/2010/02/21/multiple-curl-requests-with-php/<br><br>// build the individual requests, but do not execute them<br>$ch_1 = curl_init(&apos;http://webservice.one.com/&apos;);<br>$ch_2 = curl_init(&apos;http://webservice.two.com/&apos;);<br>curl_setopt($ch_1, CURLOPT_RETURNTRANSFER, true);<br>curl_setopt($ch_2, CURLOPT_RETURNTRANSFER, true);<br>  <br>// build the multi-curl handle, adding both $ch<br>$mh = curl_multi_init();<br>curl_multi_add_handle($mh, $ch_1);<br>curl_multi_add_handle($mh, $ch_2);<br>  <br>// execute all queries simultaneously, and continue when all are complete<br>  $running = null;<br>  do {<br>    curl_multi_exec($mh, $running);<br>  } while ($running);<br><br>//close the handles<br>curl_multi_remove_handle($mh, $ch1);<br>curl_multi_remove_handle($mh, $ch2);<br>curl_multi_close($mh);<br>  <br>// all of our requests are done, we can now access the results<br>$response_1 = curl_multi_getcontent($ch_1);<br>$response_2 = curl_multi_getcontent($ch_2);<br>echo "$response_1 $response_2"; // output results  

#

According to https://bugs.php.net/bug.php?id=61141:<br><br>On Windows setups using libcurl version 7.24 or later (which seems to correspond to PHP 5.3.10 or later), you may find that curl_multi_select() always returns -1, causing the example code in the documentation to timeout. This is, apparently, not strictly a bug: according to the libcurl documentation, you should add your own sleep if curl_multi_select returns -1.<br><br>Therefore, in order to work correctly on Windows for PHP 5.3.10+, the second loop in the example code should look something like the following:<br><br>

```
<?php
while ($active &amp;&amp; $mrc == CURLM_OK) {
    if (curl_multi_select($mh) == -1) {
        usleep(100);
    }
    do {
        $mrc = curl_multi_exec($mh, $active);
    } while ($mrc == CURLM_CALL_MULTI_PERFORM);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-init.php)

**[To root](/README.md)**