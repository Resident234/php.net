# stream_context_create



Something to keep in mind when creating SSL streams (using https://):<br><br>

```
<?php
$context = context_create_stream($context_options)
$fp = fopen('https://url', 'r', false, $context);
?>
```


One would think - the proper way to create a stream options array, would be as follows: 



```
<?php
$context_options = array (
        'https' => array (
            'method' => 'POST',
            'header'=> "Content-type: application/x-www-form-urlencoded\r\n"
                . "Content-Length: " . strlen($data) . "\r\n",
            'content' => $data
            )
        );
?>
```


THAT IS THE WRONG WAY!!!
Take notice to the 3rd line: 'https' => array (

The CORRECT way, is as follows:



```
<?php
$context_options = array (
        'http' => array (
            'method' => 'POST',
            'header'=> "Content-type: application/x-www-form-urlencoded\r\n"
                . "Content-Length: " . strlen($data) . "\r\n",
            'content' => $data
            )
        );
?>
```


Notice, the NEW 3rd line: 'http' => array (

Now - keep this in mind - I spent several hours trying to trouble shoot my issue, when I finally stumbled upon this non-documented issue.

The complete code to post to a secure page is as follows:



```
<?php
$data = array ('foo' => 'bar', 'bar' => 'baz');
$data = http_build_query($data);

$context_options = array (
        'http' => array (
            'method' => 'POST',
            'header'=> "Content-type: application/x-www-form-urlencoded\r\n"
                . "Content-Length: " . strlen($data) . "\r\n",
            'content' => $data
            )
        );

$context = context_create_stream($context_options)
$fp = fopen('https://url', 'r', false, $context);
?>
```
  

#

I big NOTE that i hope will help some one. Something that is not mentioned in the documentation, is that when php is compiled --with-curlwrappers,<br><br>So, instead of:<br><br>

```
<?php
$opts = array(
  'http'=>array(
    'method'=>"GET",
    'header'=>"Accept-language: en\r\n" .
              "Cookie: foo=bar\r\n"
  )
);

$context = stream_context_create($opts);
?>
```


You would setup the header this way: 



```
<?php
$opts = array(
  'http'=>array(
    'method'=>"GET",
    'header'=>array("Accept-language: en",
                           "Cookie: foo=bar",
                           "Custom-Header: value")
  )
);

$context = stream_context_create($opts);
?>
```
<br><br>This will work.  

#

I spent a good five hours trying to figure this out, so hopefully it will save someone else some time.<br><br>When you are trying to download a file via ftp through an HTTP proxy note that the following will not be enough:<br>

```
<?php
$opts = array('ftp' => array(
    'proxy' => 'tcp://vbinprst10:8080',
    'request_fulluri'=>true,
    'header' => array(
        "Proxy-Authorization: Basic $auth"
        )
    )
);
$context = stream_context_create($opts);
$s = file_get_contents("ftp://anonymous:anonymous@ftp.example.org",false,$context);
?>
```


Your proxy will respond that authentication is required. You may scratch your head and think "but I'm providing authentication!"

The issue is that the 'header' value is only applicable to http connections. So to authenticate on a proxy, you first have to pull a file from HTTP, before the context is valid for using on FTP.


```
<?php
$opts = array('ftp' => array(
    'proxy' => 'tcp://vbinprst10:8080',
    'request_fulluri'=>true,
    'header' => array(
        "Proxy-Authorization: Basic $auth"
        )
    ),
    'http' => array(
    'proxy' => 'tcp://vbinprst10:8080',
    'request_fulluri'=>true,
    'header' => array(
        "Proxy-Authorization: Basic $auth"
        )
    )
);
$context = stream_context_create($opts);
$s = file_get_contents("http://www.example.com",false,$context);
$s = file_get_contents("ftp://anonymous:anonymous@ftp.example.org",false,$context);
?>
```
<br><br>It&apos;s a bit roundabout, but it works. Note that the &apos;header&apos; val in the ftp array is redundant, but I kept it in anyway.  

#

I found the following code worked for me for POSTing some binary data to a remote server. I am putting it here since I could not find a quick solution to this by &apos;googling&apos; or looking through this documentation. <br><br>Disclaimer:  I have no idea if this a &apos;good&apos; solution, since I&apos;m new to PHP, but it may just suit your needs as it did mine.  I am assuming bad things will happen with very large files since the entire file is read into $fileContents. <br><br>I am using PHP 5.2.8.<br><br>   $fileHandle = fopen("someImage.jpg", "rb");<br>   $fileContents = stream_get_contents($fileHandle);<br>   fclose($fileHandle);<br><br>   $params = array(<br>      &apos;http&apos; =&gt; array<br>      (<br>          &apos;method&apos; =&gt; &apos;POST&apos;,<br>          &apos;header&apos;=&gt;"Content-Type: multipart/form-data\r\n",<br>          &apos;content&apos; =&gt; $fileContents<br>      )<br>   );<br>   $url = "http://somesite.somecompany.com?someParam=someValue";<br>   $ctx = stream_context_create($params);<br>   $fp = fopen($url, &apos;rb&apos;, false, $ctx);<br><br>   $response = stream_get_contents($fp);  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-context-create.php)

**[To root](/README.md)**