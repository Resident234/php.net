# curl_multi_select



Even after so many distro releases, in PHP v5.5.9 and libcurl v7.35.0, curl_multi_select still returns -1. The only work around here is to wait and proceed like nothing ever happened. You have to determine the "wait" required here. <br><br>In my application a very small interval like usleep(1) worked. For example:<br><br>

```
<?php
        // While we're still active, execute curl
        $active = null;
        do {
            $mrc = curl_multi_exec($multi, $active);
        } while ($mrc == CURLM_CALL_MULTI_PERFORM);
    
        while ($active &amp;&amp; $mrc == CURLM_OK) {
            // Wait for activity on any curl-connection
            if (curl_multi_select($multi) == -1) {
                usleep(1);
            }
    
            // Continue to exec until curl is ready to
            // give us more data
            do {
                $mrc = curl_multi_exec($multi, $active);
            } while ($mrc == CURLM_CALL_MULTI_PERFORM);
        }
?>
```
<br><br>Internally php curl_multi_select uses libcurl curl_multi_fdset function and its libcurl documentation says :<br>http://curl.haxx.se/libcurl/c/curl_multi_fdset.html<br><br>"When libcurl returns -1 in max_fd, it is because libcurl currently does something that isn&apos;t possible for your application to monitor with a socket and unfortunately you can then not know exactly when the current action is completed using select(). When max_fd returns with -1, you need to wait a while and then proceed and call curl_multi_perform anyway. How long to wait? I would suggest 100 milliseconds at least, but you may want to test it out in your own particular conditions to find a suitable value. <br><br>When doing select(), you should use curl_multi_timeout to figure out how long to wait for action."<br><br>Untill PHP implements curl_multi_timeout() we have to play with our application and determine the "wait".  

#

curl_multi_select($mh, $timeout) simply blocks for $timeout seconds while curl_multi_exec() returns CURLM_CALL_MULTI_PERFORM. Otherwise, it works as intended, and blocks until at least one connection has completed or $timeout seconds, whatever happens first.<br><br>For that reason, curl_multi_exec() should always be wrapped:<br><br>

```
<?php
  function full_curl_multi_exec($mh, &amp;$still_running) {
    do {
      $rv = curl_multi_exec($mh, $still_running);
    } while ($rv == CURLM_CALL_MULTI_PERFORM);
    return $rv;
  }
?>
```


With that, the core of "multi" processing becomes (ignoring error handling for brevity):



```
<?php
  full_curl_multi_exec($mh, $still_running); // start requests
  do { // "wait for completion"-loop
    curl_multi_select($mh); // non-busy (!) wait for state change
    full_curl_multi_exec($mh, $still_running); // get new state
    while ($info = curl_multi_info_read($mh)) {
      // process completed request (e.g. curl_multi_getcontent($info['handle']))
    }
  } while ($still_running);
?>
```
<br><br>Note that after starting requests, retrieval is done in the background - one of the better shots at parallel processing in PHP.  

#

This function blocks the calling process until there is activity on any of the connections opened by the curl_multi interface, or until the timeout period has expired. <br>In other words, it waits for data to be received in the opened connections.<br><br>Internally it fetches socket pointers with "curl_multi_fdset()" and runs the "select()" C function.<br>It returns in 3 cases:<br>1. Activity is detected on any socket;<br>2. Timeout has ended (second parameter);<br>3. Process received any signal (#man kill).<br><br>The function returns an integer:<br>* In case of activity it returns a number, usually 1.<br>I suppose, it returns the number of connections with activity detected.<br>* If timeout expires it returns 0<br>* In case of error it returns -1<br><br>Thanks for attention, hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-select.php)

**[To root](/README.md)**