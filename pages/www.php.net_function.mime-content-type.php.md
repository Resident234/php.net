# mime_content_type



Fast generation of uptodate mime types:<br><br>

```
<?php
define(&apos;APACHE_MIME_TYPES_URL&apos;,&apos;http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types&apos;);

function generateUpToDateMimeArray($url){
    $s=array();
    foreach(@explode("\n",@file_get_contents($url))as $x)
        if(isset($x[0])&amp;&amp;$x[0]!==&apos;#&apos;&amp;&amp;preg_match_all(&apos;#([^\s]+)#&apos;,$x,$out)&amp;&amp;isset($out[1])&amp;&amp;($c=count($out[1]))&gt;1)
            for($i=1;$i&lt;$c;$i++)
                $s[]=&apos;&amp;nbsp;&amp;nbsp;&amp;nbsp;\&apos;&apos;.$out[1][$i].&apos;\&apos; =&gt; \&apos;&apos;.$out[1][0].&apos;\&apos;&apos;;
    return @sort($s)?&apos;$mime_types = array(&lt;br /&gt;&apos;.implode($s,&apos;,&lt;br /&gt;&apos;).&apos;&lt;br /&gt;);&apos;:false;
}

echo
generateUpToDateMimeArray(APACHE_MIME_TYPES_URL);
?>
```
<br><br>Output:<br>$mime_types = array(<br>   &apos;123&apos; =&gt; &apos;application/vnd.lotus-1-2-3&apos;,<br>   &apos;3dml&apos; =&gt; &apos;text/vnd.in3d.3dml&apos;,<br>   &apos;3g2&apos; =&gt; &apos;video/3gpp2&apos;,<br>   &apos;3gp&apos; =&gt; &apos;video/3gpp&apos;,<br>   &apos;7z&apos; =&gt; &apos;application/x-7z-compressed&apos;,<br>   &apos;aab&apos; =&gt; &apos;application/x-authorware-bin&apos;,<br>   &apos;aac&apos; =&gt; &apos;audio/x-aac&apos;,<br>   &apos;aam&apos; =&gt; &apos;application/x-authorware-map&apos;,<br>   &apos;aas&apos; =&gt; &apos;application/x-authorware-seg&apos;,<br>...<br><br>Enjoy.  

#

There is a composer package that will do this:<br>https://github.com/ralouphie/mimey<br><br>

```
<?php<br>$mimes = new \Mimey\MimeTypes;<br><br>// Convert extension to MIME type:<br>$mimes-&gt;getMimeType(&apos;json&apos;); // application/json<br><br>// Convert MIME type to extension:<br>$mimes-&gt;getExtension(&apos;application/json&apos;); // json  

#

using <br>

```
<?php
function detectFileMimeType($filename=&apos;&apos;)
{
    $filename = escapeshellcmd($filename);
    $command = "file -b --mime-type -m /usr/share/misc/magic {$filename}";

    $mimeType = shell_exec($command);
            
    return trim($mimeType);
}
?>
```
<br>should work on most shared linux hosts without errors. It should also work on Windows hosts with msysgit installed.  

#



```
<?php
if(!function_exists(&apos;mime_content_type&apos;)) {

    function mime_content_type($filename) {

        $mime_types = array(

            &apos;txt&apos; =&gt; &apos;text/plain&apos;,
            &apos;htm&apos; =&gt; &apos;text/html&apos;,
            &apos;html&apos; =&gt; &apos;text/html&apos;,
            &apos;php&apos; =&gt; &apos;text/html&apos;,
            &apos;css&apos; =&gt; &apos;text/css&apos;,
            &apos;js&apos; =&gt; &apos;application/javascript&apos;,
            &apos;json&apos; =&gt; &apos;application/json&apos;,
            &apos;xml&apos; =&gt; &apos;application/xml&apos;,
            &apos;swf&apos; =&gt; &apos;application/x-shockwave-flash&apos;,
            &apos;flv&apos; =&gt; &apos;video/x-flv&apos;,

            // images
            &apos;png&apos; =&gt; &apos;image/png&apos;,
            &apos;jpe&apos; =&gt; &apos;image/jpeg&apos;,
            &apos;jpeg&apos; =&gt; &apos;image/jpeg&apos;,
            &apos;jpg&apos; =&gt; &apos;image/jpeg&apos;,
            &apos;gif&apos; =&gt; &apos;image/gif&apos;,
            &apos;bmp&apos; =&gt; &apos;image/bmp&apos;,
            &apos;ico&apos; =&gt; &apos;image/vnd.microsoft.icon&apos;,
            &apos;tiff&apos; =&gt; &apos;image/tiff&apos;,
            &apos;tif&apos; =&gt; &apos;image/tiff&apos;,
            &apos;svg&apos; =&gt; &apos;image/svg+xml&apos;,
            &apos;svgz&apos; =&gt; &apos;image/svg+xml&apos;,

            // archives
            &apos;zip&apos; =&gt; &apos;application/zip&apos;,
            &apos;rar&apos; =&gt; &apos;application/x-rar-compressed&apos;,
            &apos;exe&apos; =&gt; &apos;application/x-msdownload&apos;,
            &apos;msi&apos; =&gt; &apos;application/x-msdownload&apos;,
            &apos;cab&apos; =&gt; &apos;application/vnd.ms-cab-compressed&apos;,

            // audio/video
            &apos;mp3&apos; =&gt; &apos;audio/mpeg&apos;,
            &apos;qt&apos; =&gt; &apos;video/quicktime&apos;,
            &apos;mov&apos; =&gt; &apos;video/quicktime&apos;,

            // adobe
            &apos;pdf&apos; =&gt; &apos;application/pdf&apos;,
            &apos;psd&apos; =&gt; &apos;image/vnd.adobe.photoshop&apos;,
            &apos;ai&apos; =&gt; &apos;application/postscript&apos;,
            &apos;eps&apos; =&gt; &apos;application/postscript&apos;,
            &apos;ps&apos; =&gt; &apos;application/postscript&apos;,

            // ms office
            &apos;doc&apos; =&gt; &apos;application/msword&apos;,
            &apos;rtf&apos; =&gt; &apos;application/rtf&apos;,
            &apos;xls&apos; =&gt; &apos;application/vnd.ms-excel&apos;,
            &apos;ppt&apos; =&gt; &apos;application/vnd.ms-powerpoint&apos;,

            // open office
            &apos;odt&apos; =&gt; &apos;application/vnd.oasis.opendocument.text&apos;,
            &apos;ods&apos; =&gt; &apos;application/vnd.oasis.opendocument.spreadsheet&apos;,
        );

        $ext = strtolower(array_pop(explode(&apos;.&apos;,$filename)));
        if (array_key_exists($ext, $mime_types)) {
            return $mime_types[$ext];
        }
        elseif (function_exists(&apos;finfo_open&apos;)) {
            $finfo = finfo_open(FILEINFO_MIME);
            $mimetype = finfo_file($finfo, $filename);
            finfo_close($finfo);
            return $mimetype;
        }
        else {
            return &apos;application/octet-stream&apos;;
        }
    }
}
?>
```
  

#

Lukas V is IMO missing some point. The MIME type of a file may not be corresponding to the file suffix.<br><br>Imagine someone would obfuscate some PHP code in a .gif file, the file suffix would be &apos;GIF&apos; but the MIME would be text/plain or even text/html.<br><br>Another example is files fetched via a distant server (wget / fopen / file / fsockopen...). The server can issue an error, i.e. 404 Not Found, wich again is text/html, whatever you save the file to (download_archive.rar).<br><br>His provided function should begin by the test of the function existancy like :<br><br>function MIMEalternative($file)<br>{<br>    if(function_exists(&apos;mime_content_type&apos;))<br>        return mime_content_type($file);<br>    else<br>        return &lt;lukas_v.MIMEfunction&gt;($file);<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.mime-content-type.php)

**[To root](/README.md)**