# curl_setopt_array



In case that you need to read SSL page content from https with curl, this function can help you:<br><br>

```
<?php

function get_web_page( $url,$curl_data )
{
    $options = array(
        CURLOPT_RETURNTRANSFER =&gt; true,         // return web page
        CURLOPT_HEADER         =&gt; false,        // don&apos;t return headers
        CURLOPT_FOLLOWLOCATION =&gt; true,         // follow redirects
        CURLOPT_ENCODING       =&gt; "",           // handle all encodings
        CURLOPT_USERAGENT      =&gt; "spider",     // who am i
        CURLOPT_AUTOREFERER    =&gt; true,         // set referer on redirect
        CURLOPT_CONNECTTIMEOUT =&gt; 120,          // timeout on connect
        CURLOPT_TIMEOUT        =&gt; 120,          // timeout on response
        CURLOPT_MAXREDIRS      =&gt; 10,           // stop after 10 redirects
        CURLOPT_POST            =&gt; 1,            // i am sending post data
           CURLOPT_POSTFIELDS     =&gt; $curl_data,    // this are my post vars
        CURLOPT_SSL_VERIFYHOST =&gt; 0,            // don&apos;t verify ssl
        CURLOPT_SSL_VERIFYPEER =&gt; false,        //
        CURLOPT_VERBOSE        =&gt; 1                //
    );

    $ch      = curl_init($url);
    curl_setopt_array($ch,$options);
    $content = curl_exec($ch);
    $err     = curl_errno($ch);
    $errmsg  = curl_error($ch) ;
    $header  = curl_getinfo($ch);
    curl_close($ch);

  //  $header[&apos;errno&apos;]   = $err;
  //  $header[&apos;errmsg&apos;]  = $errmsg;
  //  $header[&apos;content&apos;] = $content;
    return $header;
}

$curl_data = "var1=60&amp;var2=test";
$url = "https://www.example.com";
$response = get_web_page($url,$curl_data);

print &apos;&lt;pre&gt;&apos;;
print_r($response);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-setopt-array.php)

**[To root](/README.md)**