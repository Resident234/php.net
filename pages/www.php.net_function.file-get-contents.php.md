# file_get_contents



Setting the timeout properly without messing with ini values:<br><br>

```
<?php
$ctx = stream_context_create(array(
    'http' => array(
        'timeout' => 1
        )
    )
);
file_get_contents("http://example.com/", 0, $ctx);
?>
```
  

---

file_get_contents can do a POST, create a context for that first:<br><br>$opts = array(&apos;http&apos; =&gt;<br>  array(<br>    &apos;method&apos;  =&gt; &apos;POST&apos;,<br>    &apos;header&apos;  =&gt; "Content-Type: text/xml\r\n".<br>      "Authorization: Basic ".base64_encode("$https_user:$https_password")."\r\n",<br>    &apos;content&apos; =&gt; $body,<br>    &apos;timeout&apos; =&gt; 60<br>  )<br>);<br>                        <br>$context  = stream_context_create($opts);<br>$url = &apos;https://&apos;.$https_server;<br>$result = file_get_contents($url, false, $context, -1, 40000);  

---

here is another (maybe the easiest) way of doing POST http requests from php using its built-in capabilities. feel free to add the headers you need (notably the Host: header) to further customize the request.<br><br>note: this method does not allow file uploads. if you want to upload a file with your request you will need to modify the context parameters to provide multipart/form-data encoding (check out http://www.php.net/manual/en/context.http.php ) and build the $data_url following the guidelines on http://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2<br><br>

```
<?php
/**
make an http POST request and return the response content and headers
@param string $url    url of the requested script
@param array $data    hash array of request variables
@return returns a hash array with response content and headers in the following form:
    array ('content'=>'<html></html>'
        , 'headers'=>array ('HTTP/1.1 200 OK', 'Connection: close', ...)
        )
*/
function http_post ($url, $data)
{
    $data_url = http_build_query ($data);
    $data_len = strlen ($data_url);

    return array ('content'=>file_get_contents ($url, false, stream_context_create (array ('http'=>array ('method'=>'POST'
            , 'header'=>"Connection: close\r\nContent-Length: $data_len\r\n"
            , 'content'=>$data_url
            ))))
        , 'headers'=>$http_response_header
        );
}
?>
```
  

---

Keep in mind that if you use a URL as the filename attribute, and the external resource is not reachable, the function will not return FALSE but instead an exception will be thrown.<br><br>So, in this case, instead of doing this:<br><br>$content = file_get_contents(&apos;https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg&apos;);<br><br>if ($content === false) {<br>    // Handle the error<br>}<br><br>Do this:<br><br>try {<br>    $content = file_get_contents(&apos;https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg&apos;);<br><br>    if ($content === false) {<br>        // Handle the error<br>    }<br>} catch (Exception $e) {<br>    // Handle exception<br>}  

---

A UTF-8 issue I&apos;ve encountered is that of reading a URL with a non-UTF-8 encoding that is later displayed improperly since file_get_contents() related to it as UTF-8. This small function should show you how to address this issue:<br><br>

```
<?php
function file_get_contents_utf8($fn) {
     $content = file_get_contents($fn);
      return mb_convert_encoding($content, 'UTF-8', 
          mb_detect_encoding($content, 'UTF-8, ISO-8859-1', true));
}
?>
```
  

---

It is important to write the method in capital letters like "GET" or "POST" and not "get" or "post". Some servers can respond a 400 error if you do not use caps in the method.  

---

Seems file looks for the file inside the current working (executing) directory before looking in the include path, even with the FILE_USE_INCLUDE_PATH flag specified.<br><br>Same behavior as include actually.<br><br>By the way I feel the doc is not entirely clear on the exact order of inclusion (see include). It seems to say the include_path is the first location to be searched, but I have come across at least one case where the directory containing the file including was actually the first to be searched.<br><br>Drat.  

---

The offset is 0 based.  Setting it to 1 will skip the first character of the stream.  

---

This is a nice and simple substitute to get_file_contents() using curl, it returns FALSE if $contents is empty.<br><br>

```
<?php
function curl_get_file_contents($URL)
    {
        $c = curl_init();
        curl_setopt($c, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($c, CURLOPT_URL, $URL);
        $contents = curl_exec($c);
        curl_close($c);

        if ($contents) return $contents;
            else return FALSE;
    }
?>
```
<br><br>Hope this help, if there is something wrong or something you don&apos;t understand let me know :)  

---

If you&apos;re having problems with binary and hex data:<br><br>I had a problem when trying to read information from a ttf, which is primarily hex data. A binary-safe file read automatically replaces byte values with their corresponding ASCII characters, so I thought that I could use the binary string when I needed readable ASCII strings, and bin2hex() when I needed hex strings.<br><br>However, this became a problem when I tried to pass those ASCII strings into other functions (namely gd functions). var_dump showed that a 5-character string contained 10 characters, but they weren&apos;t visible. A binary-to-"normal" string conversion function didn&apos;t seem to exist and I didn&apos;t want to have to convert every single character in hex using chr().<br><br>I used unpack with "c*" as the format flag to see what was going on, and found that every other character was null data (ordinal 0). To solve it, I just did<br><br>str_replace(chr(0), "", $string);<br><br>which did the trick.<br><br>This took forever to figure out so I hope this helps people reading from hex data!  

---

[Official documentation page](https://www.php.net/manual/en/function.file-get-contents.php)

**[To root](/README.md)**