# POST method uploads





I think the way an array of attachments works is kind of cumbersome. Usually the PHP guys are right on the money, but this is just counter-intuitive. It should have been more like:

Array
(
&#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; facepalm.jpg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; image/jpeg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/phpn3FmFr
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 15476
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [1] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 4
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; )
)

and not this
Array
(
&#xA0; &#xA0; [name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; facepalm.jpg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [type] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; image/jpeg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [tmp_name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; /tmp/phpn3FmFr
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [error] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 4
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [size] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 15476
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; )
)

Anyways, here is a fuller example than the sparce one in the documentation above:



```
<?php
foreach ($_FILES[&quot;attachment&quot;][&quot;error&quot;] as $key =&gt; $error)
{
&#xA0; &#xA0; &#xA0;&#xA0; $tmp_name = $_FILES[&quot;attachment&quot;][&quot;tmp_name&quot;][$key];
&#xA0; &#xA0; &#xA0;&#xA0; if (!$tmp_name) continue;

&#xA0; &#xA0; &#xA0;&#xA0; $name = basename($_FILES[&quot;attachment&quot;][&quot;name&quot;][$key]);

&#xA0; &#xA0; if ($error == UPLOAD_ERR_OK)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if ( move_uploaded_file($tmp_name, &quot;/tmp/&quot;.$name) )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $uploaded_array[] .= &quot;Uploaded file &apos;&quot;.$name.&quot;&apos;.&lt;br/&gt;\n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $errormsg .= &quot;Could not move uploaded file &apos;&quot;.$tmp_name.&quot;&apos; to &apos;&quot;.$name.&quot;&apos;&lt;br/&gt;\n&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; else $errormsg .= &quot;Upload error. [&quot;.$error.&quot;] on file &apos;&quot;.$name.&quot;&apos;&lt;br/&gt;\n&quot;;
}
?>
```



  

#



Do not use Coreywelch or Daevid&apos;s way, because their methods can handle only within two-dimensional structure. $_FILES can consist of any hierarchy, such as 3d or 4d structure.

The following example form breaks their codes:

&lt;form action=&quot;&quot; method=&quot;post&quot; enctype=&quot;multipart/form-data&quot;&gt;
&#xA0; &#xA0; &lt;input type=&quot;file&quot; name=&quot;files[x][y][z]&quot;&gt;
&#xA0; &#xA0; &lt;input type=&quot;submit&quot;&gt;
&lt;/form&gt;

As the solution, you should use PSR-7 based zendframework/zend-diactoros.

GitHub:

https://github.com/zendframework/zend-diactoros

Example:



```
<?php

use Psr\Http\Message\UploadedFileInterface;
use Zend\Diactoros\ServerRequestFactory;

$request = ServerRequestFactory::fromGlobals();

if ($request-&gt;getMethod() !== &apos;POST&apos;) {
&#xA0; &#xA0; http_response_code(405);
&#xA0; &#xA0; exit(&apos;Use POST method.&apos;);
}

$uploaded_files = $request-&gt;getUploadedFiles();

if (
&#xA0; &#xA0; !isset($uploaded_files[&apos;files&apos;][&apos;x&apos;][&apos;y&apos;][&apos;z&apos;]) ||
&#xA0; &#xA0; !$uploaded_files[&apos;files&apos;][&apos;x&apos;][&apos;y&apos;][&apos;z&apos;] instanceof UploadedFileInterface
) {
&#xA0; &#xA0; http_response_code(400);
&#xA0; &#xA0; exit(&apos;Invalid request body.&apos;);
}

$file = $uploaded_files[&apos;files&apos;][&apos;x&apos;][&apos;y&apos;][&apos;z&apos;];

if ($file-&gt;getError() !== UPLOAD_ERR_OK) {
&#xA0; &#xA0; http_response_code(400);
&#xA0; &#xA0; exit(&apos;File uploading failed.&apos;);
}

$file-&gt;moveTo(&apos;/path/to/new/file&apos;);

?>
```



  

#



The documentation doesn&apos;t have any details about how the HTML array feature formats the $_FILES array. 

Example $_FILES array:

For single file -

Array
(
&#xA0; &#xA0; [document] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; sample-file.doc
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; application/msword
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/path/phpVGCDAJ
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; )
)

Multi-files with HTML array feature -

Array
(
&#xA0; &#xA0; [documents] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; sample-file.doc
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; sample-file.doc
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; application/msword
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; application/msword
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; /tmp/path/phpVGCDAJ
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; /tmp/path/phpVGCDAJ
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; )

)

The problem occurs when you have a form that uses both single file and HTML array feature. The array isn&apos;t normalized and tends to make coding for it really sloppy. I have included a nice method to normalize the $_FILES array.



```
<?php

&#xA0; &#xA0; function normalize_files_array($files = []) {

&#xA0; &#xA0; &#xA0; &#xA0; $normalized_array = [];

&#xA0; &#xA0; &#xA0; &#xA0; foreach($files as $index =&gt; $file) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!is_array($file[&apos;name&apos;])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $normalized_array[$index][] = $file;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($file[&apos;name&apos;] as $idx =&gt; $name) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $normalized_array[$index][$idx] = [
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; $name,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;type&apos; =&gt; $file[&apos;type&apos;][$idx],
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;tmp_name&apos; =&gt; $file[&apos;tmp_name&apos;][$idx],
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;error&apos; =&gt; $file[&apos;error&apos;][$idx],
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;size&apos; =&gt; $file[&apos;size&apos;][$idx]
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return $normalized_array;

&#xA0; &#xA0; }

?>
```


The following is the output from the above method.

Array
(
&#xA0; &#xA0; [document] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; sample-file.doc
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; application/msword
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/path/phpVGCDAJ
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [documents] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; sample-file.doc
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; application/msword
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/path/phpVGCDAJ
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; sample-file.doc
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; application/msword
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/path/phpVGCDAJ
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; )

)

  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.post-method.php)

**[To root](/README.md)**