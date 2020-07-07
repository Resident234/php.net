# file_get_contents





Setting the timeout properly without messing with ini values:





```
<?php

$ctx = stream_context_create(array(

&#xA0; &#xA0; &apos;http&apos; =&gt; array(

&#xA0; &#xA0; &#xA0; &#xA0; &apos;timeout&apos; =&gt; 1

&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; )

);

file_get_contents(&quot;http://example.com/&quot;, 0, $ctx);

?>
```



  

#



file_get_contents can do a POST, create a context for that first:

$opts = array(&apos;http&apos; =&gt;
&#xA0; array(
&#xA0; &#xA0; &apos;method&apos;&#xA0; =&gt; &apos;POST&apos;,
&#xA0; &#xA0; &apos;header&apos;&#xA0; =&gt; &quot;Content-Type: text/xml\r\n&quot;.
&#xA0; &#xA0; &#xA0; &quot;Authorization: Basic &quot;.base64_encode(&quot;$https_user:$https_password&quot;).&quot;\r\n&quot;,
&#xA0; &#xA0; &apos;content&apos; =&gt; $body,
&#xA0; &#xA0; &apos;timeout&apos; =&gt; 60
&#xA0; )
);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
$context&#xA0; = stream_context_create($opts);
$url = &apos;https://&apos;.$https_server;
$result = file_get_contents($url, false, $context, -1, 40000);

  

#



here is another (maybe the easiest) way of doing POST http requests from php using its built-in capabilities. feel free to add the headers you need (notably the Host: header) to further customize the request.

note: this method does not allow file uploads. if you want to upload a file with your request you will need to modify the context parameters to provide multipart/form-data encoding (check out http://www.php.net/manual/en/context.http.php ) and build the $data_url following the guidelines on http://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2



```
<?php
/**
make an http POST request and return the response content and headers
@param string $url&#xA0; &#xA0; url of the requested script
@param array $data&#xA0; &#xA0; hash array of request variables
@return returns a hash array with response content and headers in the following form:
&#xA0; &#xA0; array (&apos;content&apos;=&gt;&apos;&lt;html&gt;&lt;/html&gt;&apos;
&#xA0; &#xA0; &#xA0; &#xA0; , &apos;headers&apos;=&gt;array (&apos;HTTP/1.1 200 OK&apos;, &apos;Connection: close&apos;, ...)
&#xA0; &#xA0; &#xA0; &#xA0; )
*/
function http_post ($url, $data)
{
&#xA0; &#xA0; $data_url = http_build_query ($data);
&#xA0; &#xA0; $data_len = strlen ($data_url);

&#xA0; &#xA0; return array (&apos;content&apos;=&gt;file_get_contents ($url, false, stream_context_create (array (&apos;http&apos;=&gt;array (&apos;method&apos;=&gt;&apos;POST&apos;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; , &apos;header&apos;=&gt;&quot;Connection: close\r\nContent-Length: $data_len\r\n&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; , &apos;content&apos;=&gt;$data_url
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ))))
&#xA0; &#xA0; &#xA0; &#xA0; , &apos;headers&apos;=&gt;$http_response_header
&#xA0; &#xA0; &#xA0; &#xA0; );
}
?>
```



  

#



Keep in mind that if you use a URL as the filename attribute, and the external resource is not reachable, the function will not return FALSE but instead an exception will be thrown.

So, in this case, instead of doing this:

$content = file_get_contents(&apos;https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg&apos;);

if ($content === false) {
&#xA0; &#xA0; // Handle the error
}

Do this:

try {
&#xA0; &#xA0; $content = file_get_contents(&apos;https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg&apos;);

&#xA0; &#xA0; if ($content === false) {
&#xA0; &#xA0; &#xA0; &#xA0; // Handle the error
&#xA0; &#xA0; }
} catch (Exception $e) {
&#xA0; &#xA0; // Handle exception
}

  

#



A UTF-8 issue I&apos;ve encountered is that of reading a URL with a non-UTF-8 encoding that is later displayed improperly since file_get_contents() related to it as UTF-8. This small function should show you how to address this issue:





```
<?php

function file_get_contents_utf8($fn) {

&#xA0; &#xA0;&#xA0; $content = file_get_contents($fn);

&#xA0; &#xA0; &#xA0; return mb_convert_encoding($content, &apos;UTF-8&apos;, 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; mb_detect_encoding($content, &apos;UTF-8, ISO-8859-1&apos;, true));

}

?>
```



  

#



It is important to write the method in capital letters like &quot;GET&quot; or &quot;POST&quot; and not &quot;get&quot; or &quot;post&quot;. Some servers can respond a 400 error if you do not use caps in the method.

  

#



Seems file looks for the file inside the current working (executing) directory before looking in the include path, even with the FILE_USE_INCLUDE_PATH flag specified.

Same behavior as include actually.

By the way I feel the doc is not entirely clear on the exact order of inclusion (see include). It seems to say the include_path is the first location to be searched, but I have come across at least one case where the directory containing the file including was actually the first to be searched.

Drat.

  

#



The offset is 0 based.&#xA0; Setting it to 1 will skip the first character of the stream.

  

#



This is a nice and simple substitute to get_file_contents() using curl, it returns FALSE if $contents is empty.



```
<?php
function curl_get_file_contents($URL)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $c = curl_init();
&#xA0; &#xA0; &#xA0; &#xA0; curl_setopt($c, CURLOPT_RETURNTRANSFER, 1);
&#xA0; &#xA0; &#xA0; &#xA0; curl_setopt($c, CURLOPT_URL, $URL);
&#xA0; &#xA0; &#xA0; &#xA0; $contents = curl_exec($c);
&#xA0; &#xA0; &#xA0; &#xA0; curl_close($c);

&#xA0; &#xA0; &#xA0; &#xA0; if ($contents) return $contents;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else return FALSE;
&#xA0; &#xA0; }
?>
```


Hope this help, if there is something wrong or something you don&apos;t understand let me know :)

  

#



If you&apos;re having problems with binary and hex data:

I had a problem when trying to read information from a ttf, which is primarily hex data. A binary-safe file read automatically replaces byte values with their corresponding ASCII characters, so I thought that I could use the binary string when I needed readable ASCII strings, and bin2hex() when I needed hex strings.

However, this became a problem when I tried to pass those ASCII strings into other functions (namely gd functions). var_dump showed that a 5-character string contained 10 characters, but they weren&apos;t visible. A binary-to-&quot;normal&quot; string conversion function didn&apos;t seem to exist and I didn&apos;t want to have to convert every single character in hex using chr().

I used unpack with &quot;c*&quot; as the format flag to see what was going on, and found that every other character was null data (ordinal 0). To solve it, I just did

str_replace(chr(0), &quot;&quot;, $string);

which did the trick.

This took forever to figure out so I hope this helps people reading from hex data!

  

#

[Official documentation page](https://www.php.net/manual/en/function.file-get-contents.php)

**[To root](/README.md)**