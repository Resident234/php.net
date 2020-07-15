# mime_content_type



Fast generation of uptodate mime types:<br><br>

```
<?php
define('APACHE_MIME_TYPES_URL','http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types');

function generateUpToDateMimeArray($url){
    $s=array();
    foreach(@explode("\n",@file_get_contents($url))as $x)
        if(isset($x[0])&amp;&amp;$x[0]!=='#'&amp;&amp;preg_match_all('#([^\s]+)#',$x,$out)&amp;&amp;isset($out[1])&amp;&amp;($c=count($out[1]))&gt;1)
            for($i=1;$i&lt;$c;$i++)
                $s[]='&amp;nbsp;&amp;nbsp;&amp;nbsp;\''.$out[1][$i].'\' => \''.$out[1][0].'\'';
    return @sort($s)?'$mime_types = array(&lt;br /&gt;'.implode($s,',&lt;br /&gt;').'&lt;br /&gt;);':false;
}

echo
generateUpToDateMimeArray(APACHE_MIME_TYPES_URL);
?>
```
<br><br>Output:<br>$mime_types = array(<br>   &apos;123&apos; =&gt; &apos;application/vnd.lotus-1-2-3&apos;,<br>   &apos;3dml&apos; =&gt; &apos;text/vnd.in3d.3dml&apos;,<br>   &apos;3g2&apos; =&gt; &apos;video/3gpp2&apos;,<br>   &apos;3gp&apos; =&gt; &apos;video/3gpp&apos;,<br>   &apos;7z&apos; =&gt; &apos;application/x-7z-compressed&apos;,<br>   &apos;aab&apos; =&gt; &apos;application/x-authorware-bin&apos;,<br>   &apos;aac&apos; =&gt; &apos;audio/x-aac&apos;,<br>   &apos;aam&apos; =&gt; &apos;application/x-authorware-map&apos;,<br>   &apos;aas&apos; =&gt; &apos;application/x-authorware-seg&apos;,<br>...<br><br>Enjoy.  

#

There is a composer package that will do this:<br>https://github.com/ralouphie/mimey<br><br>

```
<?php
$mimes = new \Mimey\MimeTypes;

// Convert extension to MIME type:
$mimes->getMimeType('json'); // application/json

// Convert MIME type to extension:
$mimes->getExtension('application/json'); // json?>
```
  

#

using <br>

```
<?php
function detectFileMimeType($filename='')
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
if(!function_exists('mime_content_type')) {

    function mime_content_type($filename) {

        $mime_types = array(

            'txt' => 'text/plain',
            'htm' => 'text/html',
            'html' => 'text/html',
            'php' => 'text/html',
            'css' => 'text/css',
            'js' => 'application/javascript',
            'json' => 'application/json',
            'xml' => 'application/xml',
            'swf' => 'application/x-shockwave-flash',
            'flv' => 'video/x-flv',

            // images
            'png' => 'image/png',
            'jpe' => 'image/jpeg',
            'jpeg' => 'image/jpeg',
            'jpg' => 'image/jpeg',
            'gif' => 'image/gif',
            'bmp' => 'image/bmp',
            'ico' => 'image/vnd.microsoft.icon',
            'tiff' => 'image/tiff',
            'tif' => 'image/tiff',
            'svg' => 'image/svg+xml',
            'svgz' => 'image/svg+xml',

            // archives
            'zip' => 'application/zip',
            'rar' => 'application/x-rar-compressed',
            'exe' => 'application/x-msdownload',
            'msi' => 'application/x-msdownload',
            'cab' => 'application/vnd.ms-cab-compressed',

            // audio/video
            'mp3' => 'audio/mpeg',
            'qt' => 'video/quicktime',
            'mov' => 'video/quicktime',

            // adobe
            'pdf' => 'application/pdf',
            'psd' => 'image/vnd.adobe.photoshop',
            'ai' => 'application/postscript',
            'eps' => 'application/postscript',
            'ps' => 'application/postscript',

            // ms office
            'doc' => 'application/msword',
            'rtf' => 'application/rtf',
            'xls' => 'application/vnd.ms-excel',
            'ppt' => 'application/vnd.ms-powerpoint',

            // open office
            'odt' => 'application/vnd.oasis.opendocument.text',
            'ods' => 'application/vnd.oasis.opendocument.spreadsheet',
        );

        $ext = strtolower(array_pop(explode('.',$filename)));
        if (array_key_exists($ext, $mime_types)) {
            return $mime_types[$ext];
        }
        elseif (function_exists('finfo_open')) {
            $finfo = finfo_open(FILEINFO_MIME);
            $mimetype = finfo_file($finfo, $filename);
            finfo_close($finfo);
            return $mimetype;
        }
        else {
            return 'application/octet-stream';
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