# curl_setopt



Please everyone, stop setting CURLOPT_SSL_VERIFYPEER to false or 0. If your PHP installation doesn&apos;t have an up-to-date CA root certificate bundle, download the one at the curl website and save it on your server:<br><br>http://curl.haxx.se/docs/caextract.html<br><br>Then set a path to it in your php.ini file, e.g. on Windows:<br><br>curl.cainfo=c:\php\cacert.pem<br><br>Turning off CURLOPT_SSL_VERIFYPEER allows man in the middle (MITM) attacks, which you don&apos;t want!  

#

It is important that anyone working with cURL and PHP keep in mind that not all of the CURLOPT and CURLINFO constants are documented. I always recommend reading the cURL documentation directly as it sometimes contains better information. The cURL API in tends to be fubar as well so do not expect things to be where you would normally logically look for them.<br><br>curl is especially difficult to work with when it comes to cookies. So I will talk about what I found with PHP 5.6 and curl 7.26.<br><br>If you want to manage cookies in memory without using files including reading, writing and clearing custom cookies then continue reading.<br><br>To start with, the way to enable in memory only cookies associated with a cURL handle you should use:<br><br>    curl_setopt($curl, CURLOPT_COOKIEFILE, "");<br><br>cURL likes to use magic strings in options as special commands. Rather than having an option to enable the cookie engine in memory it uses a magic string to do that. Although vaguely the documentation here mentions this however most people like me wouldn&apos;t even read that because a COOKIEFILE is the complete opposite of what we want.<br><br>To get the cookies for a curl handle you can use:<br><br>    curl_getinfo($curl, CURLINFO_COOKIELIST);<br><br>This will give an array containing a string for each cookie. It is tab delimited and unfortunately you will have to parse it yourself if you want to do anything beyond copying the cookies.<br><br>To clear the in memory cookies for a cURL handle you can use:<br><br>    curl_setopt($curl, CURLOPT_COOKIELIST, "ALL");<br><br>This is a magic string. There are others in the cURL documentation. If a magic string isn&apos;t used, this field should take a cookie in the same string format as in getinfo for the cookielist constant. This can be used to delete individual cookies although it&apos;s not the most elegant API for doing so.<br><br>For copying cookies I recommend using curl_share_init.<br><br>You can also copy cookies from one handle to another like so:<br><br>    foreach(curl_getinfo($curl_a, CURLINFO_COOKIELIST) as $cookie_line)<br>        curl_setopt($curl, CURLOPT_COOKIELIST, $cookie_line);<br><br>An inelegant way to delete a cookie would be to skip the one you don&apos;t want. <br><br>I only recommend using COOKIELIST with magic strings because the cookie format is not secure or stable. You can inject tabs into at least path and name so it becomes impossible to parse reliably. If you must parse this then to keep it secure I recommend prohibiting more than 6 tabs in the content which probably isn&apos;t a big loss to most people.<br><br>A the absolute minimum for validation I would suggest:<br><br>    /^([^\t]+\t){5}[^\t]+$/D<br><br>Here is the format:<br><br>    #define SEP  "\t"  /* Tab separates the fields */<br> <br>    char *my_cookie =<br>      "example.com"    /* Hostname */<br>      SEP "FALSE"      /* Include subdomains */<br>      SEP "/"          /* Path */<br>      SEP "FALSE"      /* Secure */<br>      SEP "0"          /* Expiry in epoch time format. 0 == Session */<br>      SEP "foo"        /* Name */<br>      SEP "bar";       /* Value */  

#

Clarification on the callback methods:<br><br>- CURLOPT_HEADERFUNCTION is for handling header lines received *in the response*,<br>- CURLOPT_WRITEFUNCTION is for handling data received *from the response*,<br>- CURLOPT_READFUNCTION is for handling data passed along *in the request*.<br><br>The callback "string" can be any callable function, that includes the array(&amp;$obj, &apos;someMethodName&apos;) format.<br><br> -Philippe  

#

PUT requests are very simple, just make sure to specify a content-length header and set post fields as a string.<br><br>Example:<br><br>

```
<?php
function doPut($url, $fields)
{
   $fields = (is_array($fields)) ? http_build_query($fields) : $fields;

   if($ch = curl_init($url))
   {
      curl_setopt($ch, CURLOPT_CUSTOMREQUEST, &apos;PUT&apos;);
      curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
      curl_setopt($ch, CURLOPT_HTTPHEADER, array(&apos;Content-Length: &apos; . strlen($fields)));
      curl_setopt($ch, CURLOPT_POSTFIELDS, $fields);
      curl_exec($ch);

      $status = curl_getinfo($ch, CURLINFO_HTTP_CODE);

      curl_close($ch);

      return (int) $status;
   }
   else
   {
      return false;
   }
}

if(doPut(&apos;http://example.com/api/a/b/c&apos;, array(&apos;foo&apos; =&gt; &apos;bar&apos;)) == 200)
   // do something
else
   // do something else.
?>
```


You can grab the request data on the other side with:



```
<?php
if($_SERVER[&apos;REQUEST_METHOD&apos;] == &apos;PUT&apos;)
{
   parse_str(file_get_contents(&apos;php://input&apos;), $requestData);

   // Array ( [foo] =&gt; bar )
   print_r($requestData);

   // Do something with data...
}
?>
```
<br><br>DELETE  can be done in exactly the same way.  

#

If you are doing a POST, and the content length is 1,025 or greater, then curl exploits a feature of http 1.1: 100 (Continue) Status.<br><br>See http://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8.2.3<br><br>* it adds a header, "Expect: 100-continue".  <br>* it then sends the request head, waits for a 100 response code, then sends the content <br><br>Not all web servers support this though.  Various errors are returned depending on the server.  If this happens to you, suppress the "Expect" header with this command:<br><br>

```
<?php
curl_setopt($ch, CURLOPT_HTTPHEADER, array(&apos;Expect:&apos;));
?>
```
<br><br>See http://www.gnegg.ch/2007/02/the-return-of-except-100-continue/  

#

If you want to Curl to follow redirects and you would also like Curl to echo back any cookies that are set in the process, use this:<br><br>

```
<?php curl_setopt($ch, CURLOPT_COOKIEJAR, &apos;-&apos;); ?>
```
<br><br>&apos;-&apos; means stdout<br><br>-dw  

#

if you would like to send xml request to a server (lets say, making a soap proxy),<br>you have to set<br><br>

```
<?php
curl_setopt($ch, CURLOPT_HTTPHEADER, Array("Content-Type: text/xml"));
?>
```


makesure you watch for cache issue:
the below code will prevent cache...



```
<?php
curl_setopt($ch, CURLOPT_FORBID_REUSE, 1);
curl_setopt($ch, CURLOPT_FRESH_CONNECT, 1);
?>
```
<br><br>hope it helps ;)  

#

I spent a couple of days trying to POST a multi-dimensional array of form fields, including a file upload, to a remote server to update a product. Here are the breakthroughs that FINALLY allowed the script to run as desired.<br><br>Firstly, the HTML form used input names like these:<br>&lt;input type="text" name="product[name]" /&gt;<br>&lt;input type="text" name="product[cost]" /&gt;<br>&lt;input type="file" name="product[thumbnail]" /&gt;<br>in conjunction with two other form inputs not part of the product array<br>&lt;input type="text" name="method" value="put" /&gt;<br>&lt;input type="text" name="mode" /&gt;<br><br>I used several cURL options, but the only two (other than URL) that mattered were:<br>curl_setopt($handle, CURLOPT_POST, true);<br>curl_setopt($handle, CURLOPT_POSTFIELDS, $postfields);<br>Pretty standard so far.<br>Note: headers didn&apos;t need to be set, cURL automatically sets headers (like content-type: multipart/form-data; content-length...) when you pass an array into CURLOPT_POSTFIELDS.<br>Note: even though this is supposed to be a PUT command through an HTTP POST form, no special PUT options needed to be passed natively through cURL. Options such as<br>curl_setopt($handle, CURLOPT_HTTPHEADER, array(&apos;X-HTTP-Method-Override: PUT&apos;, &apos;Content-Length: &apos; . strlen($fields)));<br>or<br>curl_setopt($handle, CURLOPT_PUT, true);<br>or<br>curl_setopt($handle, CURLOPT_CUSTOMREQUEST, "PUT);<br>were not needed to make the code work.<br><br>The fields I wanted to pass through cURL were arranged into an array something like this:<br>$postfields = array("method" =&gt; $_POST["method"],<br>                    "mode" =&gt; $_POST["mode"],<br>                    "product" =&gt; array("name" =&gt; $_POST["product"], <br>                                        "cost" =&gt; $_POST["product"]["cost"], <br>                                        "thumbnail" =&gt; "@{$_FILES["thumbnail"]["tmp_name"]};type={$_FILES["thumbnail"]["type"]}")<br>                    );<br><br>-Notice how the @ precedes the temporary filename, this creates a link so PHP will upload/transfer an actual file instead of just the file name, which would happen if the @ isn&apos;t included.<br>-Notice how I forcefully set the mime-type of the file to upload. I was having issues where images filetypes were defaulting to octet-stream instead of image/png or image/jpeg or whatever the type of the selected image.<br><br>I then tried passing $postfields straight into curl_setopt($this-&gt;handle, CURLOPT_POSTFIELDS, $postfields); but it didn&apos;t work.<br>I tried using http_build_query($postfields); but that didn&apos;t work properly either.<br>In both cases either the file wouldn&apos;t be treated as an actual file and the form data wasn&apos;t being sent properly. The problem was HTTP&apos;s methods of transmitting arrays. While PHP and other languages can figure out how to handle arrays passed via forms, HTTP isn&apos;t quite as sofisticated. I had to rewrite the $postfields array like so:<br>$postfields = array("method" =&gt; $_POST["method"],<br>                    "mode" =&gt; $_POST["mode"],<br>                    "product[name]" =&gt; $_POST["product"], <br>                    "product[cost]" =&gt; $_POST["product"]["cost"], <br>                    "product[thumbnail]" =&gt; "@{$_FILES["thumbnail"]["tmp_name"]}");<br>curl_setopt($handle, CURLOPT_POSTFIELDS, $postfields);<br><br>This, without the use of http_build_query, solved all of my problems. Now the receiving host outputs both $_POST and $_FILES vars correctly.  

#

If you want cURL to timeout in less than one second, you can use CURLOPT_TIMEOUT_MS, although there is a bug/"feature"  on "Unix-like systems" that causes libcurl to timeout immediately if the value is &lt; 1000 ms with the error "cURL Error (28): Timeout was reached".  The explanation for this behavior is:<br><br>"If libcurl is built to use the standard system name resolver, that portion of the transfer will still use full-second resolution for timeouts with a minimum timeout allowed of one second."<br><br>What this means to PHP developers is "You can use this function without testing it first, because you can&apos;t tell if libcurl is using the standard system name resolver (but you can be pretty sure it is)"<br><br>The problem is that on (Li|U)nix, when libcurl uses the standard name resolver, a SIGALRM is raised during name resolution which libcurl thinks is the timeout alarm.<br><br>The solution is to disable signals using CURLOPT_NOSIGNAL.  Here&apos;s an example script that requests itself causing a 10-second delay so you can test timeouts:<br><br>

```
<?php
if (!isset($_GET[&apos;foo&apos;])) {
        // Client
        $ch = curl_init(&apos;http://localhost/test/test_timeout.php?foo=bar&apos;);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_NOSIGNAL, 1);
        curl_setopt($ch, CURLOPT_TIMEOUT_MS, 200);
        $data = curl_exec($ch);
        $curl_errno = curl_errno($ch);
        $curl_error = curl_error($ch);
        curl_close($ch);

        if ($curl_errno &gt; 0) {
                echo "cURL Error ($curl_errno): $curl_error\n";
        } else {
                echo "Data received: $data\n";
        }
} else {
        // Server
        sleep(10);
        echo "Done.";
}
?>
```
  

#

If you wish to find the size of the file you are streaming and use it as your header this is how:<br><br>

```
<?php

function write_function($curl_resource, $string)
{
    if(curl_getinfo($curl_resource, CURLINFO_SIZE_DOWNLOAD) &lt;= 2000)
    {
        header(&apos;Expires: 0&apos;);
        header(&apos;Cache-Control: must-revalidate, post-check=0, pre-check=0&apos;);
        header(&apos;Pragma: public&apos;);
        header(&apos;Content-Description: File Transfer&apos;);
        header("Content-Transfer-Encoding: binary");
        header("Content-Type: ".curl_getinfo($curl_resource, CURLINFO_CONTENT_TYPE)."");
        header("Content-Length: ".curl_getinfo($curl_resource, CURLINFO_CONTENT_LENGTH_DOWNLOAD)."");
    }
    
    print $string;

    return mb_strlen($string, &apos;8bit&apos;);
}

?>
```
<br><br>1440 is the the default number of bytes curl will call the write function (BUFFERSIZE does not affect this, i actually think you can not change this value), so it means the headers are going to be set only one time.<br><br>write_function must return the exact number of bytes of the string, so you can return a value with mb_strlen.  

#

Many hosters use PHP safe_mode or/and open_basedir, so you can&apos;t use CURLOPT_FOLLOWLOCATION. If you try, you see message like this:<br>CURLOPT_FOLLOWLOCATION cannot be activated when safe_mode is enabled or an open_basedir is set in [you script name &amp; path] on line XXX<br><br>First, I try to use zsalab function (http://us2.php.net/manual/en/function.curl-setopt.php#102121) from this page, but for some reason it did not work properly. So, I wrote my own.<br><br>It can be use instead of curl_exec. If server HTTP response codes is 30x, function will forward the request as long as the response is not different from 30x (for example, 200 Ok). Also you can use POST.<br><br>function curlExec(/* Array */$curlOptions=&apos;&apos;, /* Array */$curlHeaders=&apos;&apos;, /* Array */$postFields=&apos;&apos;)<br>{<br>  $newUrl = &apos;&apos;;<br>  $maxRedirection = 10;<br>  do<br>  {<br>    if ($maxRedirection&lt;1) die(&apos;Error: reached the limit of redirections&apos;);<br><br>    $ch = curl_init();<br>    if (!empty($curlOptions)) curl_setopt_array($ch, $curlOptions);<br>    if (!empty($curlHeaders)) curl_setopt($ch, CURLOPT_HTTPHEADER, $curlHeaders);<br>    if (!empty($postFields))<br>    {<br>      curl_setopt($ch, CURLOPT_POST, 1);<br>      curl_setopt($ch, CURLOPT_POSTFIELDS, $postFields);<br>    }<br>    <br>    if (!empty($newUrl)) curl_setopt($ch, CURLOPT_URL, $newUrl); // redirect needed<br>    <br>    $curlResult = curl_exec($ch);<br>    $code = curl_getinfo($ch, CURLINFO_HTTP_CODE);<br><br>    if ($code == 301 || $code == 302 || $code == 303 || $code == 307)<br>    {<br>      preg_match(&apos;/Location:(.*?)\n/&apos;, $curlResult, $matches);<br>      $newUrl = trim(array_pop($matches));<br>      curl_close($ch);<br><br>      $maxRedirection--;<br>      continue;<br>    }<br>    else // no more redirection<br>    {<br>      $code = 0;<br>      curl_close($ch);<br>    }<br>  }<br>  while($code);<br>  return $curlResult;<br>}  

#

When CURLOPT_FOLLOWLOCATION and CURLOPT_HEADER are both true and redirect/s have happened then the header returned by curl_exec() will contain all the headers in the redirect chain in the order they were encountered.  

#

I&apos;ve found that setting CURLOPT_HTTPHEADER more than once will clear out any headers you&apos;ve set previously with CURLOPT_HTTPHEADER.<br><br>Consider the following:<br>

```
<?php
    # ...

    curl_setopt($cURL,CURLOPT_HTTPHEADER,array (
        "Content-Type: text/xml; charset=utf-8",
        "Expect: 100-continue"
    ));

    # ... do some other stuff ...

    curl_setopt($cURL,CURLOPT_HTTPHEADER,array (
        "Accept: application/json"
    ));

    # ...
?>
```
<br><br>Both the Content-Type and Expect I set will not be in the outgoing headers, but Accept will.  

#

After much struggling, I managed to get a SOAP request requiring HTTP authentication to work.  Here&apos;s some source that will hopefully be useful to others. <br><br>         

```
<?php

         $credentials = "username:password";
         
         // Read the XML to send to the Web Service
         $request_file = "./SampleRequest.xml";
        $fh = fopen($request_file, &apos;r&apos;);
        $xml_data = fread($fh, filesize($request_file));
        fclose($fh);
                
        $url = "http://www.example.com/services/calculation";
        $page = "/services/calculation";
        $headers = array(
            "POST ".$page." HTTP/1.0",
            "Content-type: text/xml;charset=\"utf-8\"",
            "Accept: text/xml",
            "Cache-Control: no-cache",
            "Pragma: no-cache",
            "SOAPAction: \"run\"",
            "Content-length: ".strlen($xml_data),
            "Authorization: Basic " . base64_encode($credentials)
        );
       
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL,$url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($ch, CURLOPT_TIMEOUT, 60);
        curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
        curl_setopt($ch, CURLOPT_USERAGENT, $defined_vars[&apos;HTTP_USER_AGENT&apos;]);
        
        // Apply the XML to our curl call
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $xml_data); 

        $data = curl_exec($ch); 

        if (curl_errno($ch)) {
            print "Error: " . curl_error($ch);
        } else {
            // Show me the result
            var_dump($data);
            curl_close($ch);
        }

?>
```
  

#

It appears that setting CURLOPT_FILE before setting CURLOPT_RETURNTRANSFER doesn&apos;t work, presumably because CURLOPT_FILE depends on CURLOPT_RETURNTRANSFER being set.<br><br>So do this:<br><br>

```
<?php
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_FILE, $fp);
?>
```


not this:



```
<?php
curl_setopt($ch, CURLOPT_FILE, $fp);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
?>
```
  

#

Sometimes you can&apos;t use CURLOPT_COOKIEJAR and CURLOPT_COOKIEFILE becoz of the server php-settings(They say u may grab any files from server using these options). Here is the solution<br>1)Don&apos;t use CURLOPT_FOLLOWLOCATION<br>2)Use curl_setopt($ch, CURLOPT_HEADER, 1)<br>3)Grab from the header cookies like this:<br>preg_match_all(&apos;|Set-Cookie: (.*);|U&apos;, $content, $results);    <br>$cookies = implode(&apos;;&apos;, $results[1]);<br>4)Set them using curl_setopt($ch, CURLOPT_COOKIE,  $cookies);<br><br>Good Luck, Yevgen  

#

If you&apos;re getting trouble with cookie handling in curl: <br><br>- curl manages tranparently cookies in a single curl session<br>- the option <br>

```
<?php curl_setopt($ch, CURLOPT_COOKIEJAR, "/tmp/cookieFileName"); ?>
```


makes curl to store the cookies in a file at the and of the curl session

- the option 


```
<?php curl_setopt($ch, CURLOPT_COOKIEFILE, "/tmp/cookieFileName"); ?>
```


makes curl to use the given file as source for the cookies to send to the server. 

so to handle correctly cookies between different curl session, the you have to do something like this:



```
<?php
       $ch = curl_init(); 
       curl_setopt ($ch, CURLOPT_URL, $url); 
       curl_setopt ($ch, CURLOPT_COOKIEJAR, COOKIE_FILE_PATH);
       curl_setopt ($ch, CURLOPT_COOKIEFILE, COOKIE_FILE_PATH);

       curl_setopt ($ch, CURLOPT_RETURNTRANSFER, 1); 
       $result = curl_exec ($ch);
       curl_close($ch);
       return $result;
?>
```
<br><br>in particular this is NECESSARY if you are using PEAR_SOAP libraries to build a webservice client over https and the remote server need to establish a session cookie. in fact each soap message is sent using a different curl session!!<br><br>I hope this can help someone<br>Luca  

#

Handling redirections with curl if safe_mode or open_basedir is enabled. The function working transparent, no problem with header and returntransfer options. You can handle the max redirection with the optional second argument (the function is set the variable to zero if max redirection exceeded).<br>Second parameter values:<br>- maxredirect is null or not set: redirect maximum five time, after raise PHP warning<br>- maxredirect is greather then zero: no raiser error, but parameter variable set to zero<br>- maxredirect is less or equal zero: no follow redirections<br><br>

```
<?php
function curl_exec_follow(/*resource*/ $ch, /*int*/ &amp;$maxredirect = null) {
    $mr = $maxredirect === null ? 5 : intval($maxredirect);
    if (ini_get(&apos;open_basedir&apos;) == &apos;&apos; &amp;&amp; ini_get(&apos;safe_mode&apos; == &apos;Off&apos;)) {
        curl_setopt($ch, CURLOPT_FOLLOWLOCATION, $mr &gt; 0);
        curl_setopt($ch, CURLOPT_MAXREDIRS, $mr);
    } else {
        curl_setopt($ch, CURLOPT_FOLLOWLOCATION, false);
        if ($mr &gt; 0) {
            $newurl = curl_getinfo($ch, CURLINFO_EFFECTIVE_URL);

            $rch = curl_copy_handle($ch);
            curl_setopt($rch, CURLOPT_HEADER, true);
            curl_setopt($rch, CURLOPT_NOBODY, true);
            curl_setopt($rch, CURLOPT_FORBID_REUSE, false);
            curl_setopt($rch, CURLOPT_RETURNTRANSFER, true);
            do {
                curl_setopt($rch, CURLOPT_URL, $newurl);
                $header = curl_exec($rch);
                if (curl_errno($rch)) {
                    $code = 0;
                } else {
                    $code = curl_getinfo($rch, CURLINFO_HTTP_CODE);
                    if ($code == 301 || $code == 302) {
                        preg_match(&apos;/Location:(.*?)\n/&apos;, $header, $matches);
                        $newurl = trim(array_pop($matches));
                    } else {
                        $code = 0;
                    }
                }
            } while ($code &amp;&amp; --$mr);
            curl_close($rch);
            if (!$mr) {
                if ($maxredirect === null) {
                    trigger_error(&apos;Too many redirects. When following redirects, libcurl hit the maximum amount.&apos;, E_USER_WARNING);
                } else {
                    $maxredirect = 0;
                }
                return false;
            }
            curl_setopt($ch, CURLOPT_URL, $newurl);
        }
    }
    return curl_exec($ch);
}
?>
```
  

#

CURLOPT_POST must be left unset if you want the Content-Type header set to "multipart/form-data" (e.g., when CURLOPT_POSTFIELDS is an array). If you set CURLOPT_POSTFIELDS to an array and have CURLOPT_POST set to TRUE, Content-Length will be -1 and most sane servers will reject the request. If you set CURLOPT_POSTFIELDS to an array and have CURLOPT_POST set to FALSE, cURL will send a GET request.  

#

Passing in PHP&apos;s $_SESSION into your cURL call:<br><br>

```
<?php
session_start();
$strCookie = &apos;PHPSESSID=&apos; . $_COOKIE[&apos;PHPSESSID&apos;] . &apos;; path=/&apos;;
session_write_close();

$curl_handle = curl_init(&apos;enter_external_url_here&apos;);
curl_setopt( $curl_handle, CURLOPT_COOKIE, $strCookie );
curl_exec($curl_handle);
curl_close($curl_handle);
?>
```
<br><br>This worked great for me.  I was calling pages from the same server and needed to keep the $_SESSION variables.  This passes them over.  If you want to test, just print_r($_SESSION);<br><br>Enjoy!  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-setopt.php)

**[To root](/README.md)**