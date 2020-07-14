# curl_multi_getcontent



This seems to work as expected for me - allowing me to get the content from a curl_multi operation into variables :<br><br>(Thanks go to many other notes in related documentation (there is much copy/pasting) all I did was add the relevant line(s))<br><br>&lt;?<br>    $aURLs = array("http://www.php.net","http://www.w3cschools.com"); // array of URLs<br>    $mh = curl_multi_init(); // init the curl Multi<br>    <br>    $aCurlHandles = array(); // create an array for the individual curl handles<br><br>    foreach ($aURLs as $id=&gt;$url) { //add the handles for each url<br>        $ch = curl_setup($url,$socks5_proxy,$usernamepass);<br>        $ch = curl_init(); // init curl, and then setup your options<br>        curl_setopt($ch, CURLOPT_URL, $url);<br>        curl_setopt($ch, CURLOPT_RETURNTRANSFER,1); // returns the result - very important<br>        curl_setopt($ch, CURLOPT_HEADER, 0); // no headers in the output<br><br>        $aCurlHandles[$url] = $ch;<br>        curl_multi_add_handle($mh,$ch);<br>    }<br>    <br>    $active = null;<br>    //execute the handles<br>    do {<br>        $mrc = curl_multi_exec($mh, $active);<br>    } <br>    while ($mrc == CURLM_CALL_MULTI_PERFORM);<br><br>    while ($active &amp;&amp; $mrc == CURLM_OK) {<br>        if (curl_multi_select($mh) != -1) {<br>            do {<br>                $mrc = curl_multi_exec($mh, $active);<br>            } while ($mrc == CURLM_CALL_MULTI_PERFORM);<br>        }<br>    }<br>    <br>/* This is the relevant bit */<br>        // iterate through the handles and get your content<br>    foreach ($aCurlHandles as $url=&gt;$ch) {<br>        $html = curl_multi_getcontent($ch); // get the content<br>                // do what you want with the HTML<br>        curl_multi_remove_handle($mh, $ch); // remove the handle (assuming  you are done with it);<br>    }<br>/* End of the relevant bit */<br><br>    curl_multi_close($mh); // close the curl multi handler<br><br>?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-getcontent.php)

**[To root](/README.md)**