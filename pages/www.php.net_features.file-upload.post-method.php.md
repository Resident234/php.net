# POST method uploads



I think the way an array of attachments works is kind of cumbersome. Usually the PHP guys are right on the money, but this is just counter-intuitive. It should have been more like:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [name] =&gt; facepalm.jpg<br>            [type] =&gt; image/jpeg<br>            [tmp_name] =&gt; /tmp/phpn3FmFr<br>            [error] =&gt; 0<br>            [size] =&gt; 15476<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [name] =&gt; <br>            [type] =&gt; <br>            [tmp_name] =&gt; <br>            [error] =&gt; 4<br>            [size] =&gt; <br>        )<br>)<br><br>and not this<br>Array<br>(<br>    [name] =&gt; Array<br>        (<br>            [0] =&gt; facepalm.jpg<br>            [1] =&gt; <br>        )<br><br>    [type] =&gt; Array<br>        (<br>            [0] =&gt; image/jpeg<br>            [1] =&gt; <br>        )<br><br>    [tmp_name] =&gt; Array<br>        (<br>            [0] =&gt; /tmp/phpn3FmFr<br>            [1] =&gt; <br>        )<br><br>    [error] =&gt; Array<br>        (<br>            [0] =&gt; 0<br>            [1] =&gt; 4<br>        )<br><br>    [size] =&gt; Array<br>        (<br>            [0] =&gt; 15476<br>            [1] =&gt; 0<br>        )<br>)<br><br>Anyways, here is a fuller example than the sparce one in the documentation above:<br><br>

```
<?php
foreach ($_FILES["attachment"]["error"] as $key => $error)
{
       $tmp_name = $_FILES["attachment"]["tmp_name"][$key];
       if (!$tmp_name) continue;

       $name = basename($_FILES["attachment"]["name"][$key]);

    if ($error == UPLOAD_ERR_OK)
    {
        if ( move_uploaded_file($tmp_name, "/tmp/".$name) )
            $uploaded_array[] .= "Uploaded file '".$name."'.&lt;br/&gt;\n";
        else
            $errormsg .= "Could not move uploaded file '".$tmp_name."' to '".$name."'&lt;br/&gt;\n";
    }
    else $errormsg .= "Upload error. [".$error."] on file '".$name."'&lt;br/&gt;\n";
}
?>
```
  

#

Do not use Coreywelch or Daevid&apos;s way, because their methods can handle only within two-dimensional structure. $_FILES can consist of any hierarchy, such as 3d or 4d structure.<br><br>The following example form breaks their codes:<br><br>&lt;form action="" method="post" enctype="multipart/form-data"&gt;<br>    &lt;input type="file" name="files[x][y][z]"&gt;<br>    &lt;input type="submit"&gt;<br>&lt;/form&gt;<br><br>As the solution, you should use PSR-7 based zendframework/zend-diactoros.<br><br>GitHub:<br><br>https://github.com/zendframework/zend-diactoros<br><br>Example:<br><br>

```
<?php

use Psr\Http\Message\UploadedFileInterface;
use Zend\Diactoros\ServerRequestFactory;

$request = ServerRequestFactory::fromGlobals();

if ($request->getMethod() !== 'POST') {
    http_response_code(405);
    exit('Use POST method.');
}

$uploaded_files = $request->getUploadedFiles();

if (
    !isset($uploaded_files['files']['x']['y']['z']) ||
    !$uploaded_files['files']['x']['y']['z'] instanceof UploadedFileInterface
) {
    http_response_code(400);
    exit('Invalid request body.');
}

$file = $uploaded_files['files']['x']['y']['z'];

if ($file->getError() !== UPLOAD_ERR_OK) {
    http_response_code(400);
    exit('File uploading failed.');
}

$file->moveTo('/path/to/new/file');

?>
```
  

#

The documentation doesn&apos;t have any details about how the HTML array feature formats the $_FILES array. <br><br>Example $_FILES array:<br><br>For single file -<br><br>Array<br>(<br>    [document] =&gt; Array<br>        (<br>            [name] =&gt; sample-file.doc<br>            [type] =&gt; application/msword<br>            [tmp_name] =&gt; /tmp/path/phpVGCDAJ<br>            [error] =&gt; 0<br>            [size] =&gt; 0<br>        )<br>)<br><br>Multi-files with HTML array feature -<br><br>Array<br>(<br>    [documents] =&gt; Array<br>        (<br>            [name] =&gt; Array<br>                (<br>                    [0] =&gt; sample-file.doc<br>                    [1] =&gt; sample-file.doc<br>                )<br><br>            [type] =&gt; Array<br>                (<br>                    [0] =&gt; application/msword<br>                    [1] =&gt; application/msword<br>                )<br><br>            [tmp_name] =&gt; Array<br>                (<br>                    [0] =&gt; /tmp/path/phpVGCDAJ<br>                    [1] =&gt; /tmp/path/phpVGCDAJ<br>                )<br><br>            [error] =&gt; Array<br>                (<br>                    [0] =&gt; 0<br>                    [1] =&gt; 0<br>                )<br><br>            [size] =&gt; Array<br>                (<br>                    [0] =&gt; 0<br>                    [1] =&gt; 0<br>                )<br><br>        )<br><br>)<br><br>The problem occurs when you have a form that uses both single file and HTML array feature. The array isn&apos;t normalized and tends to make coding for it really sloppy. I have included a nice method to normalize the $_FILES array.<br><br>

```
<?php

    function normalize_files_array($files = []) {

        $normalized_array = [];

        foreach($files as $index => $file) {

            if (!is_array($file['name'])) {
                $normalized_array[$index][] = $file;
                continue;
            }

            foreach($file['name'] as $idx => $name) {
                $normalized_array[$index][$idx] = [
                    'name' => $name,
                    'type' => $file['type'][$idx],
                    'tmp_name' => $file['tmp_name'][$idx],
                    'error' => $file['error'][$idx],
                    'size' => $file['size'][$idx]
                ];
            }

        }

        return $normalized_array;

    }

?>
```
<br><br>The following is the output from the above method.<br><br>Array<br>(<br>    [document] =&gt; Array<br>        (<br>            [0] =&gt; Array<br>                (<br>                [name] =&gt; sample-file.doc<br>                    [type] =&gt; application/msword<br>                    [tmp_name] =&gt; /tmp/path/phpVGCDAJ<br>                    [error] =&gt; 0<br>                    [size] =&gt; 0<br>                )<br><br>        )<br><br>    [documents] =&gt; Array<br>        (<br>            [0] =&gt; Array<br>                (<br>                    [name] =&gt; sample-file.doc<br>                    [type] =&gt; application/msword<br>                    [tmp_name] =&gt; /tmp/path/phpVGCDAJ<br>                    [error] =&gt; 0<br>                    [size] =&gt; 0<br>                )<br><br>            [1] =&gt; Array<br>                (<br>                    [name] =&gt; sample-file.doc<br>                    [type] =&gt; application/msword<br>                    [tmp_name] =&gt; /tmp/path/phpVGCDAJ<br>                    [error] =&gt; 0<br>                    [size] =&gt; 0<br>                )<br><br>        )<br><br>)  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.post-method.php)

**[To root](/README.md)**