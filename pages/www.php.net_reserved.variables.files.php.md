# $_FILES



see http://php.net/manual/en/features.file-upload.post-method.php for documentation of the $_FILES array, which is what I came to this page for in the first place.  

---

If you are looking for the $_FILES[&apos;error&apos;] code explanations, be sure to read:<br><br>Handling File Uploads - Error Messages Explained<br>http://www.php.net/manual/en/features.file-upload.errors.php  

---

A note of security: Don&apos;t ever trust $_FILES["image"]["type"]. It takes whatever is sent from the browser, so don&apos;t trust this for the image type.  I recommend using finfo_open (http://www.php.net/manual/en/function.finfo-open.php) to verify the MIME type of a file. It will parse the MAGIC in the file and return it&apos;s type...this can be trusted (you can also use the "file" program on Unix, but I would refrain from ever making a System call with your PHP code...that&apos;s just asking for problems).  

---

The format of this array is (assuming your form has two input type=file fields named "file1", "file2", etc):<br><br>Array<br>(<br>    [file1] =&gt; Array<br>        (<br>            [name] =&gt; MyFile.txt (comes from the browser, so treat as tainted)<br>            [type] =&gt; text/plain  (not sure where it gets this from - assume the browser, so treat as tainted)<br>            [tmp_name] =&gt; /tmp/php/php1h4j1o (could be anywhere on your system, depending on your config settings, but the user has no control, so this isn&apos;t tainted)<br>            [error] =&gt; UPLOAD_ERR_OK  (= 0)<br>            [size] =&gt; 123   (the size in bytes)<br>        )<br><br>    [file2] =&gt; Array<br>        (<br>            [name] =&gt; MyFile.jpg<br>            [type] =&gt; image/jpeg<br>            [tmp_name] =&gt; /tmp/php/php6hst32<br>            [error] =&gt; UPLOAD_ERR_OK<br>            [size] =&gt; 98174<br>        )<br>)<br><br>Last I checked (a while ago now admittedly), if you use array parameters in your forms (that is, form names ending in square brackets, like several file fields called "download[file1]", "download[file2]" etc), then the array format becomes... interesting.<br><br>Array<br>(<br>    [download] =&gt; Array<br>        (<br>            [name] =&gt; Array<br>                (<br>                    [file1] =&gt; MyFile.txt<br>                    [file2] =&gt; MyFile.jpg<br>                )<br><br>            [type] =&gt; Array<br>                (<br>                    [file1] =&gt; text/plain<br>                    [file2] =&gt; image/jpeg<br>                )<br><br>            [tmp_name] =&gt; Array<br>                (<br>                    [file1] =&gt; /tmp/php/php1h4j1o<br>                    [file2] =&gt; /tmp/php/php6hst32<br>                )<br><br>            [error] =&gt; Array<br>                (<br>                    [file1] =&gt; UPLOAD_ERR_OK<br>                    [file2] =&gt; UPLOAD_ERR_OK<br>                )<br><br>            [size] =&gt; Array<br>                (<br>                    [file1] =&gt; 123<br>                    [file2] =&gt; 98174<br>                )<br>        )<br>)<br><br>So you&apos;d need to access the error param of file1 as, eg $_Files[&apos;download&apos;][&apos;error&apos;][&apos;file1&apos;]  

---

A nice trick to reorder the $_FILES array when you use a input name as array is:<br><br>

```
<?php
function diverse_array($vector) {
    $result = array();
    foreach($vector as $key1 => $value1)
        foreach($value1 as $key2 => $value2)
            $result[$key2][$key1] = $value2;
    return $result;
}
?>
```


will transform this:

array(1) {
    ["upload"]=>array(2) {
        ["name"]=>array(2) {
            [0]=>string(9)"file0.txt"
            [1]=>string(9)"file1.txt"
        }
        ["type"]=>array(2) {
            [0]=>string(10)"text/plain"
            [1]=>string(10)"text/html"
        }
    }
}

into:

array(1) {
    ["upload"]=>array(2) {
        [0]=>array(2) {
            ["name"]=>string(9)"file0.txt"
            ["type"]=>string(10)"text/plain"
        },
        [1]=>array(2) {
            ["name"]=>string(9)"file1.txt"
            ["type"]=>string(10)"text/html"
        }
    }
}

just do:



```
<?php $upload = diverse_array($_FILES["upload"]); ?>
```
  

---

If $_FILES is empty, even when uploading, try adding enctype="multipart/form-data" to the form tag and make sure you have file uploads turned on.  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.files.php)

**[To root](/README.md)**