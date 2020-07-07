# $_FILES





see http://php.net/manual/en/features.file-upload.post-method.php for documentation of the $_FILES array, which is what I came to this page for in the first place.

  

#



If you are looking for the $_FILES[&apos;error&apos;] code explanations, be sure to read:

Handling File Uploads - Error Messages Explained
http://www.php.net/manual/en/features.file-upload.errors.php

  

#



A note of security: Don&apos;t ever trust $_FILES[&quot;image&quot;][&quot;type&quot;]. It takes whatever is sent from the browser, so don&apos;t trust this for the image type.&#xA0; I recommend using finfo_open (http://www.php.net/manual/en/function.finfo-open.php) to verify the MIME type of a file. It will parse the MAGIC in the file and return it&apos;s type...this can be trusted (you can also use the &quot;file&quot; program on Unix, but I would refrain from ever making a System call with your PHP code...that&apos;s just asking for problems).

  

#



The format of this array is (assuming your form has two input type=file fields named &quot;file1&quot;, &quot;file2&quot;, etc):

Array
(
&#xA0; &#xA0; [file1] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; MyFile.txt (comes from the browser, so treat as tainted)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; text/plain&#xA0; (not sure where it gets this from - assume the browser, so treat as tainted)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/php/php1h4j1o (could be anywhere on your system, depending on your config settings, but the user has no control, so this isn&apos;t tainted)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; UPLOAD_ERR_OK&#xA0; (= 0)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 123&#xA0;&#xA0; (the size in bytes)
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [file2] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; MyFile.jpg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; image/jpeg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/php/php6hst32
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; UPLOAD_ERR_OK
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 98174
&#xA0; &#xA0; &#xA0; &#xA0; )
)

Last I checked (a while ago now admittedly), if you use array parameters in your forms (that is, form names ending in square brackets, like several file fields called &quot;download[file1]&quot;, &quot;download[file2]&quot; etc), then the array format becomes... interesting.

Array
(
&#xA0; &#xA0; [download] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; MyFile.txt
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; MyFile.jpg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; text/plain
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; image/jpeg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; /tmp/php/php1h4j1o
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; /tmp/php/php6hst32
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; UPLOAD_ERR_OK
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; UPLOAD_ERR_OK
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; 123
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; 98174
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; )
)

So you&apos;d need to access the error param of file1 as, eg $_Files[&apos;download&apos;][&apos;error&apos;][&apos;file1&apos;]

  

#



A nice trick to reorder the $_FILES array when you use a input name as array is:





```
<?php

function diverse_array($vector) {

&#xA0; &#xA0; $result = array();

&#xA0; &#xA0; foreach($vector as $key1 =&gt; $value1)

&#xA0; &#xA0; &#xA0; &#xA0; foreach($value1 as $key2 =&gt; $value2)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result[$key2][$key1] = $value2;

&#xA0; &#xA0; return $result;

}

?>
```




will transform this:



array(1) {

&#xA0; &#xA0; [&quot;upload&quot;]=&gt;array(2) {

&#xA0; &#xA0; &#xA0; &#xA0; [&quot;name&quot;]=&gt;array(2) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0]=&gt;string(9)&quot;file0.txt&quot;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1]=&gt;string(9)&quot;file1.txt&quot;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; [&quot;type&quot;]=&gt;array(2) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0]=&gt;string(10)&quot;text/plain&quot;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1]=&gt;string(10)&quot;text/html&quot;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}



into:



array(1) {

&#xA0; &#xA0; [&quot;upload&quot;]=&gt;array(2) {

&#xA0; &#xA0; &#xA0; &#xA0; [0]=&gt;array(2) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;name&quot;]=&gt;string(9)&quot;file0.txt&quot;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;type&quot;]=&gt;string(10)&quot;text/plain&quot;

&#xA0; &#xA0; &#xA0; &#xA0; },

&#xA0; &#xA0; &#xA0; &#xA0; [1]=&gt;array(2) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;name&quot;]=&gt;string(9)&quot;file1.txt&quot;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;type&quot;]=&gt;string(10)&quot;text/html&quot;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}



just do:





```
<?php $upload = diverse_array($_FILES[&quot;upload&quot;]); ?>
```



  

#



If $_FILES is empty, even when uploading, try adding enctype=&quot;multipart/form-data&quot; to the form tag and make sure you have file uploads turned on.

  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.files.php)

**[To root](/README.md)**