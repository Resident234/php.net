# curl_exec



Just in case anyone is looking for a a couple of simple functions [to help automate cURL processes for POST and GET queries] I thought I&apos;d post these.<br><br>

```
<?php

/**
 * Send a POST requst using cURL
 * @param string $url to request
 * @param array $post values to send
 * @param array $options for cURL
 * @return string
 */
function curl_post($url, array $post = NULL, array $options = array())
{
    $defaults = array(
        CURLOPT_POST =&gt; 1,
        CURLOPT_HEADER =&gt; 0,
        CURLOPT_URL =&gt; $url,
        CURLOPT_FRESH_CONNECT =&gt; 1,
        CURLOPT_RETURNTRANSFER =&gt; 1,
        CURLOPT_FORBID_REUSE =&gt; 1,
        CURLOPT_TIMEOUT =&gt; 4,
        CURLOPT_POSTFIELDS =&gt; http_build_query($post)
    );

    $ch = curl_init();
    curl_setopt_array($ch, ($options + $defaults));
    if( ! $result = curl_exec($ch))
    {
        trigger_error(curl_error($ch));
    }
    curl_close($ch);
    return $result;
}

/**
 * Send a GET requst using cURL
 * @param string $url to request
 * @param array $get values to send
 * @param array $options for cURL
 * @return string
 */
function curl_get($url, array $get = NULL, array $options = array())
{    
    $defaults = array(
        CURLOPT_URL =&gt; $url. (strpos($url, &apos;?&apos;) === FALSE ? &apos;?&apos; : &apos;&apos;). http_build_query($get),
        CURLOPT_HEADER =&gt; 0,
        CURLOPT_RETURNTRANSFER =&gt; TRUE,
        CURLOPT_TIMEOUT =&gt; 4
    );
    
    $ch = curl_init();
    curl_setopt_array($ch, ($options + $defaults));
    if( ! $result = curl_exec($ch))
    {
        trigger_error(curl_error($ch));
    }
    curl_close($ch);
    return $result;
}
?>
```
  

#

Don&apos;t disable SSL verification! You don&apos;t need to, and it&apos;s super easy to stay secure! If you found that turning off "CURLOPT_SSL_VERIFYHOST" and "CURLOPT_SSL_VERIFYPEER" solved your problem, odds are you&apos;re just on a Windows box. Takes 2 min to solve the problem. Walkthrough here:<br><br>https://snippets.webaware.com.au/howto/stop-turning-off-curlopt_ssl_verifypeer-and-fix-your-php-config/  

#

Be careful when using curl_exec() and the CURLOPT_RETURNTRANSFER option. According to the manual and assorted documentation:<br>Set CURLOPT_RETURNTRANSFER to TRUE to return the transfer as a string of the return value of curl_exec() instead of outputting it out directly.<br><br>When retrieving a document with no content (ie. 0 byte file), curl_exec() will return bool(true), not an empty string. I&apos;ve not seen any mention of this in the manual.<br><br>Example code to reproduce this:<br>

```
<?php

    // fictional URL to an existing file with no data in it (ie. 0 byte file)
    $url = &apos;http://www.example.com/empty_file.txt&apos;;

    $curl = curl_init();
    
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($curl, CURLOPT_HEADER, false);

    // execute and return string (this should be an empty string &apos;&apos;)
    $str = curl_exec($curl);

    curl_close($curl);

    // the value of $str is actually bool(true), not empty string &apos;&apos;
    var_dump($str);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-exec.php)

**[To root](/README.md)**