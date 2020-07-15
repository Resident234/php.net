# Uploading multiple files



When uploading multiple files, the $_FILES variable is created in the form:<br><br>Array<br>(<br>    [name] =&gt; Array<br>        (<br>            [0] =&gt; foo.txt<br>            [1] =&gt; bar.txt<br>        )<br><br>    [type] =&gt; Array<br>        (<br>            [0] =&gt; text/plain<br>            [1] =&gt; text/plain<br>        )<br><br>    [tmp_name] =&gt; Array<br>        (<br>            [0] =&gt; /tmp/phpYzdqkD<br>            [1] =&gt; /tmp/phpeEwEWG<br>        )<br><br>    [error] =&gt; Array<br>        (<br>            [0] =&gt; 0<br>            [1] =&gt; 0<br>        )<br><br>    [size] =&gt; Array<br>        (<br>            [0] =&gt; 123<br>            [1] =&gt; 456<br>        )<br>)<br><br>I found it made for a little cleaner code if I had the uploaded files array in the form<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [name] =&gt; foo.txt<br>            [type] =&gt; text/plain<br>            [tmp_name] =&gt; /tmp/phpYzdqkD<br>            [error] =&gt; 0<br>            [size] =&gt; 123<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [name] =&gt; bar.txt<br>            [type] =&gt; text/plain<br>            [tmp_name] =&gt; /tmp/phpeEwEWG<br>            [error] =&gt; 0<br>            [size] =&gt; 456<br>        )<br>)<br><br>I wrote a quick function that would convert the $_FILES array to the cleaner (IMHO) array.<br><br>

```
<?php

function reArrayFiles(&amp;$file_post) {

    $file_ary = array();
    $file_count = count($file_post['name']);
    $file_keys = array_keys($file_post);

    for ($i=0; $i<$file_count; $i++) {
        foreach ($file_keys as $key) {
            $file_ary[$i][$key] = $file_post[$key][$i];
        }
    }

    return $file_ary;
}

?>
```


Now I can do the following:



```
<?php

if ($_FILES['upload']) {
    $file_ary = reArrayFiles($_FILES['ufile']);

    foreach ($file_ary as $file) {
        print 'File Name: ' . $file['name'];
        print 'File Type: ' . $file['type'];
        print 'File Size: ' . $file['size'];
    }
}

?>
```
  

#

This is also needed for &lt;input type=file multiple&gt; elements.<br><br>So, if you have an input element like this:<br>&lt;input type="file" multiple="multiple" name="foobar" /&gt;<br>This should be written as<br>&lt;input type="file" multiple="multiple" name="foobar[]" /&gt;<br>else you&apos;ll only be able to get one of the files.  

#

The cleanest way to rearrange the $_FILES<br><br>

```
<?php
function rearrange( $arr ){
    foreach( $arr as $key => $all ){
        foreach( $all as $i => $val ){
            $new[$i][$key] = $val;    
        }    
    }
    return $new;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.multiple.php)

**[To root](/README.md)**