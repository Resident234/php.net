# mime_content_type





Fast generation of uptodate mime types:



```
<?php
define(&apos;APACHE_MIME_TYPES_URL&apos;,&apos;http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types&apos;);

function generateUpToDateMimeArray($url){
&#xA0; &#xA0; $s=array();
&#xA0; &#xA0; foreach(@explode(&quot;\n&quot;,@file_get_contents($url))as $x)
&#xA0; &#xA0; &#xA0; &#xA0; if(isset($x[0])&amp;&amp;$x[0]!==&apos;#&apos;&amp;&amp;preg_match_all(&apos;#([^\s]+)#&apos;,$x,$out)&amp;&amp;isset($out[1])&amp;&amp;($c=count($out[1]))&gt;1)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for($i=1;$i&lt;$c;$i++)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $s[]=&apos;&amp;nbsp;&amp;nbsp;&amp;nbsp;\&apos;&apos;.$out[1][$i].&apos;\&apos; =&gt; \&apos;&apos;.$out[1][0].&apos;\&apos;&apos;;
&#xA0; &#xA0; return @sort($s)?&apos;$mime_types = array(&lt;br /&gt;&apos;.implode($s,&apos;,&lt;br /&gt;&apos;).&apos;&lt;br /&gt;);&apos;:false;
}

echo
generateUpToDateMimeArray(APACHE_MIME_TYPES_URL);
?>
```


Output:
$mime_types = array(
&#xA0;&#xA0; &apos;123&apos; =&gt; &apos;application/vnd.lotus-1-2-3&apos;,
&#xA0;&#xA0; &apos;3dml&apos; =&gt; &apos;text/vnd.in3d.3dml&apos;,
&#xA0;&#xA0; &apos;3g2&apos; =&gt; &apos;video/3gpp2&apos;,
&#xA0;&#xA0; &apos;3gp&apos; =&gt; &apos;video/3gpp&apos;,
&#xA0;&#xA0; &apos;7z&apos; =&gt; &apos;application/x-7z-compressed&apos;,
&#xA0;&#xA0; &apos;aab&apos; =&gt; &apos;application/x-authorware-bin&apos;,
&#xA0;&#xA0; &apos;aac&apos; =&gt; &apos;audio/x-aac&apos;,
&#xA0;&#xA0; &apos;aam&apos; =&gt; &apos;application/x-authorware-map&apos;,
&#xA0;&#xA0; &apos;aas&apos; =&gt; &apos;application/x-authorware-seg&apos;,
...

Enjoy.

  

#



There is a composer package that will do this:
https://github.com/ralouphie/mimey



```
<?php
$mimes = new \Mimey\MimeTypes;

// Convert extension to MIME type:
$mimes-&gt;getMimeType(&apos;json&apos;); // application/json

// Convert MIME type to extension:
$mimes-&gt;getExtension(&apos;application/json&apos;); // json


  

#



using 


```
<?php
function detectFileMimeType($filename=&apos;&apos;)
{
&#xA0; &#xA0; $filename = escapeshellcmd($filename);
&#xA0; &#xA0; $command = &quot;file -b --mime-type -m /usr/share/misc/magic {$filename}&quot;;

&#xA0; &#xA0; $mimeType = shell_exec($command);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; return trim($mimeType);
}
?>
```

should work on most shared linux hosts without errors. It should also work on Windows hosts with msysgit installed.

  

#





```
<?php
if(!function_exists(&apos;mime_content_type&apos;)) {

&#xA0; &#xA0; function mime_content_type($filename) {

&#xA0; &#xA0; &#xA0; &#xA0; $mime_types = array(

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;txt&apos; =&gt; &apos;text/plain&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;htm&apos; =&gt; &apos;text/html&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;html&apos; =&gt; &apos;text/html&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;php&apos; =&gt; &apos;text/html&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;css&apos; =&gt; &apos;text/css&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;js&apos; =&gt; &apos;application/javascript&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;json&apos; =&gt; &apos;application/json&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;xml&apos; =&gt; &apos;application/xml&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;swf&apos; =&gt; &apos;application/x-shockwave-flash&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;flv&apos; =&gt; &apos;video/x-flv&apos;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // images
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;png&apos; =&gt; &apos;image/png&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;jpe&apos; =&gt; &apos;image/jpeg&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;jpeg&apos; =&gt; &apos;image/jpeg&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;jpg&apos; =&gt; &apos;image/jpeg&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;gif&apos; =&gt; &apos;image/gif&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;bmp&apos; =&gt; &apos;image/bmp&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ico&apos; =&gt; &apos;image/vnd.microsoft.icon&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;tiff&apos; =&gt; &apos;image/tiff&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;tif&apos; =&gt; &apos;image/tiff&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;svg&apos; =&gt; &apos;image/svg+xml&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;svgz&apos; =&gt; &apos;image/svg+xml&apos;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // archives
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;zip&apos; =&gt; &apos;application/zip&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;rar&apos; =&gt; &apos;application/x-rar-compressed&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;exe&apos; =&gt; &apos;application/x-msdownload&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;msi&apos; =&gt; &apos;application/x-msdownload&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;cab&apos; =&gt; &apos;application/vnd.ms-cab-compressed&apos;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // audio/video
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;mp3&apos; =&gt; &apos;audio/mpeg&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;qt&apos; =&gt; &apos;video/quicktime&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;mov&apos; =&gt; &apos;video/quicktime&apos;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // adobe
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;pdf&apos; =&gt; &apos;application/pdf&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;psd&apos; =&gt; &apos;image/vnd.adobe.photoshop&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ai&apos; =&gt; &apos;application/postscript&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;eps&apos; =&gt; &apos;application/postscript&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ps&apos; =&gt; &apos;application/postscript&apos;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ms office
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;doc&apos; =&gt; &apos;application/msword&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;rtf&apos; =&gt; &apos;application/rtf&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;xls&apos; =&gt; &apos;application/vnd.ms-excel&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ppt&apos; =&gt; &apos;application/vnd.ms-powerpoint&apos;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // open office
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;odt&apos; =&gt; &apos;application/vnd.oasis.opendocument.text&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ods&apos; =&gt; &apos;application/vnd.oasis.opendocument.spreadsheet&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; );

&#xA0; &#xA0; &#xA0; &#xA0; $ext = strtolower(array_pop(explode(&apos;.&apos;,$filename)));
&#xA0; &#xA0; &#xA0; &#xA0; if (array_key_exists($ext, $mime_types)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $mime_types[$ext];
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; elseif (function_exists(&apos;finfo_open&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $finfo = finfo_open(FILEINFO_MIME);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $mimetype = finfo_file($finfo, $filename);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; finfo_close($finfo);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $mimetype;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;application/octet-stream&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}
?>
```



  

#



Lukas V is IMO missing some point. The MIME type of a file may not be corresponding to the file suffix.

Imagine someone would obfuscate some PHP code in a .gif file, the file suffix would be &apos;GIF&apos; but the MIME would be text/plain or even text/html.

Another example is files fetched via a distant server (wget / fopen / file / fsockopen...). The server can issue an error, i.e. 404 Not Found, wich again is text/html, whatever you save the file to (download_archive.rar).

His provided function should begin by the test of the function existancy like :

function MIMEalternative($file)
{
&#xA0; &#xA0; if(function_exists(&apos;mime_content_type&apos;))
&#xA0; &#xA0; &#xA0; &#xA0; return mime_content_type($file);
&#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; return &lt;lukas_v.MIMEfunction&gt;($file);
}

  

#

[Official documentation page](https://www.php.net/manual/en/function.mime-content-type.php)

**[To root](/README.md)**