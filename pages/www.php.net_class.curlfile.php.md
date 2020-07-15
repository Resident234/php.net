# The CURLFile class



There are "@" issue on multipart POST requests.<br><br>Solution for PHP 5.5 or later:<br>- Enable CURLOPT_SAFE_UPLOAD.<br>- Use CURLFile instead of "@".<br><br>Solution for PHP 5.4 or earlier:<br>- Build up multipart content body by youself.<br>- Change "Content-Type" header by yourself.<br><br>The following snippet will help you :D<br><br>

```
<?php

/**
 * For safe multipart POST request for PHP5.3 ~ PHP 5.4.
 * 
 * @param resource $ch cURL resource
 * @param array $assoc "name => value"
 * @param array $files "name => path"
 * @return bool
 */
function curl_custom_postfields($ch, array $assoc = array(), array $files = array()) {
    
    // invalid characters for "name" and "filename"
    static $disallow = array("\0", "\"", "\r", "\n");
    
    // build normal parameters
    foreach ($assoc as $k => $v) {
        $k = str_replace($disallow, "_", $k);
        $body[] = implode("\r\n", array(
            "Content-Disposition: form-data; name=\"{$k}\"",
            "",
            filter_var($v), 
        ));
    }
    
    // build file parameters
    foreach ($files as $k => $v) {
        switch (true) {
            case false === $v = realpath(filter_var($v)):
            case !is_file($v):
            case !is_readable($v):
                continue; // or return false, throw new InvalidArgumentException
        }
        $data = file_get_contents($v);
        $v = call_user_func("end", explode(DIRECTORY_SEPARATOR, $v));
        $k = str_replace($disallow, "_", $k);
        $v = str_replace($disallow, "_", $v);
        $body[] = implode("\r\n", array(
            "Content-Disposition: form-data; name=\"{$k}\"; filename=\"{$v}\"",
            "Content-Type: application/octet-stream",
            "",
            $data, 
        ));
    }
    
    // generate safe boundary 
    do {
        $boundary = "---------------------" . md5(mt_rand() . microtime());
    } while (preg_grep("/{$boundary}/", $body));
    
    // add boundary for each parameters
    array_walk($body, function (&amp;$part) use ($boundary) {
        $part = "--{$boundary}\r\n{$part}";
    });
    
    // add final boundary
    $body[] = "--{$boundary}--";
    $body[] = "";
    
    // set options
    return @curl_setopt_array($ch, array(
        CURLOPT_POST       => true,
        CURLOPT_POSTFIELDS => implode("\r\n", $body),
        CURLOPT_HTTPHEADER => array(
            "Expect: 100-continue",
            "Content-Type: multipart/form-data; boundary={$boundary}", // change Content-Type
        ),
    ));
}

?>
```
  

#

i&apos;ve seen some downvotes , here is a small example of using curl to upload image .<br><br>

```
<?php
$target="http://youraddress.tld/example/upload.php";

# http://php.net/manual/en/curlfile.construct.php

// Create a CURLFile object / procedural method 
$cfile = curl_file_create('resource/test.png','image/png','testpic'); // try adding 

// Create a CURLFile object / oop method 
#$cfile = new CURLFile('resource/test.png','image/png','testpic'); // uncomment and use if the upper procedural method is not working.

// Assign POST data
$imgdata = array('myimage' => $cfile);

$curl = curl_init();
curl_setopt($curl, CURLOPT_URL, $target);
curl_setopt($curl, CURLOPT_USERAGENT,'Opera/9.80 (Windows NT 6.2; Win64; x64) Presto/2.12.388 Version/12.15');
curl_setopt($curl, CURLOPT_HTTPHEADER,array('User-Agent: Opera/9.80 (Windows NT 6.2; Win64; x64) Presto/2.12.388 Version/12.15','Referer: http://someaddress.tld','Content-Type: multipart/form-data'));
curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false); // stop verifying certificate
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true); 
curl_setopt($curl, CURLOPT_POST, true); // enable posting
curl_setopt($curl, CURLOPT_POSTFIELDS, $imgdata); // post images 
curl_setopt($curl, CURLOPT_FOLLOWLOCATION, true); // if any redirection after upload
$r = curl_exec($curl); 
curl_close($curl);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.curlfile.php)

**[To root](/README.md)**