# Uploading multiple files





When uploading multiple files, the $_FILES variable is created in the form:

Array
(
&#xA0; &#xA0; [name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; foo.txt
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; bar.txt
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [type] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; text/plain
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; text/plain
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [tmp_name] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; /tmp/phpYzdqkD
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; /tmp/phpeEwEWG
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [error] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [size] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 123
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 456
&#xA0; &#xA0; &#xA0; &#xA0; )
)

I found it made for a little cleaner code if I had the uploaded files array in the form

Array
(
&#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; foo.txt
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; text/plain
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/phpYzdqkD
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 123
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [1] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; bar.txt
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; text/plain
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/phpeEwEWG
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 456
&#xA0; &#xA0; &#xA0; &#xA0; )
)

I wrote a quick function that would convert the $_FILES array to the cleaner (IMHO) array.



```
<?php

function reArrayFiles(&amp;$file_post) {

&#xA0; &#xA0; $file_ary = array();
&#xA0; &#xA0; $file_count = count($file_post[&apos;name&apos;]);
&#xA0; &#xA0; $file_keys = array_keys($file_post);

&#xA0; &#xA0; for ($i=0; $i&lt;$file_count; $i++) {
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($file_keys as $key) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $file_ary[$i][$key] = $file_post[$key][$i];
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; return $file_ary;
}

?>
```


Now I can do the following:



```
<?php

if ($_FILES[&apos;upload&apos;]) {
&#xA0; &#xA0; $file_ary = reArrayFiles($_FILES[&apos;ufile&apos;]);

&#xA0; &#xA0; foreach ($file_ary as $file) {
&#xA0; &#xA0; &#xA0; &#xA0; print &apos;File Name: &apos; . $file[&apos;name&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; print &apos;File Type: &apos; . $file[&apos;type&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; print &apos;File Size: &apos; . $file[&apos;size&apos;];
&#xA0; &#xA0; }
}

?>
```



  

#



This is also needed for &lt;input type=file multiple&gt; elements.

So, if you have an input element like this:
&lt;input type=&quot;file&quot; multiple=&quot;multiple&quot; name=&quot;foobar&quot; /&gt;
This should be written as
&lt;input type=&quot;file&quot; multiple=&quot;multiple&quot; name=&quot;foobar[]&quot; /&gt;
else you&apos;ll only be able to get one of the files.

  

#



The cleanest way to rearrange the $_FILES



```
<?php
function rearrange( $arr ){
&#xA0; &#xA0; foreach( $arr as $key =&gt; $all ){
&#xA0; &#xA0; &#xA0; &#xA0; foreach( $all as $i =&gt; $val ){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new[$i][$key] = $val;&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; 
&#xA0; &#xA0; }
&#xA0; &#xA0; return $new;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.multiple.php)

**[To root](/README.md)**