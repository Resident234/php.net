# Error Messages Explained





Update to Adams old comment.

This is probably useful to someone.



```
<?php

$phpFileUploadErrors = array(
&#xA0; &#xA0; 0 =&gt; &apos;There is no error, the file uploaded with success&apos;,
&#xA0; &#xA0; 1 =&gt; &apos;The uploaded file exceeds the upload_max_filesize directive in php.ini&apos;,
&#xA0; &#xA0; 2 =&gt; &apos;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&apos;,
&#xA0; &#xA0; 3 =&gt; &apos;The uploaded file was only partially uploaded&apos;,
&#xA0; &#xA0; 4 =&gt; &apos;No file was uploaded&apos;,
&#xA0; &#xA0; 6 =&gt; &apos;Missing a temporary folder&apos;,
&#xA0; &#xA0; 7 =&gt; &apos;Failed to write file to disk.&apos;,
&#xA0; &#xA0; 8 =&gt; &apos;A PHP extension stopped the file upload.&apos;,
);


  

#



[EDIT BY danbrown AT php DOT net: This code is a fixed version of a note originally submitted by (Thalent, Michiel Thalen) on 04-Mar-2009.]





This is a handy exception to use when handling upload errors:





```
<?php



class UploadException extends Exception

{

&#xA0; &#xA0; public function __construct($code) {

&#xA0; &#xA0; &#xA0; &#xA0; $message = $this-&gt;codeToMessage($code);

&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct($message, $code);

&#xA0; &#xA0; }



&#xA0; &#xA0; private function codeToMessage($code)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; switch ($code) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_INI_SIZE:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;The uploaded file exceeds the upload_max_filesize directive in php.ini&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_FORM_SIZE:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_PARTIAL:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;The uploaded file was only partially uploaded&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_NO_FILE:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;No file was uploaded&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_NO_TMP_DIR:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;Missing a temporary folder&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_CANT_WRITE:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;Failed to write file to disk&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_EXTENSION:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;File upload stopped by extension&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; default:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;Unknown upload error&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return $message;

&#xA0; &#xA0; }

}



// Use

 if ($_FILES[&apos;file&apos;][&apos;error&apos;] === UPLOAD_ERR_OK) {

//uploading successfully done

} else {

throw new UploadException($_FILES[&apos;file&apos;][&apos;error&apos;]);

}

?>
```



  

#



This is probably useful to someone.





```
<?php

array(

&#xA0; &#xA0; &#xA0; &#xA0; 0=&gt;&quot;There is no error, the file uploaded with success&quot;, 

&#xA0; &#xA0; &#xA0; &#xA0; 1=&gt;&quot;The uploaded file exceeds the upload_max_filesize directive in php.ini&quot;, 

&#xA0; &#xA0; &#xA0; &#xA0; 2=&gt;&quot;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&quot;

&#xA0; &#xA0; &#xA0; &#xA0; 3=&gt;&quot;The uploaded file was only partially uploaded&quot;,

&#xA0; &#xA0; &#xA0; &#xA0; 4=&gt;&quot;No file was uploaded&quot;,

&#xA0; &#xA0; &#xA0; &#xA0; 6=&gt;&quot;Missing a temporary folder&quot; 

);

?>
```



  

#



if post is greater than post_max_size set in php.ini



$_FILES and $_POST will return empty

  

#



One thing that is annoying is that the way these constant values are handled requires processing no error with the equality, which wastes a little bit of space.&#xA0; Even though &quot;no error&quot; is 0, which typically evaluates to &quot;false&quot; in an if statement, it will always evaluate to true in this context.



So, instead of this:

-----



```
<?php

if($_FILES[&apos;userfile&apos;][&apos;error&apos;]) {

&#xA0; // handle the error

} else {

&#xA0; // process

}

?>
```


-----

You have to do this:

-----



```
<?php

if($_FILES[&apos;userfile&apos;][&apos;error&apos;]==0) {

&#xA0; // process

} else {

&#xA0; // handle the error

}

?>
```


-----

Also, ctype_digit fails, but is_int works.&#xA0; If you&apos;re wondering... no, it doesn&apos;t make any sense.



To Schoschie:



You ask the question:&#xA0; Why make stuff complicated when you can make it easy?&#xA0; I ask the same question since the version of the code you / Anonymous / Thalent (per danbrown) have posted is unnecessary overhead and would result in a function call, as well as a potentially lengthy switch statement.&#xA0; In a loop, that would be deadly... try this instead:



-----



```
<?php

$error_types = array(

1=&gt;&apos;The uploaded file exceeds the upload_max_filesize directive in php.ini.&apos;,

&apos;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form.&apos;,

&apos;The uploaded file was only partially uploaded.&apos;,

&apos;No file was uploaded.&apos;,

6=&gt;&apos;Missing a temporary folder.&apos;,

&apos;Failed to write file to disk.&apos;,

&apos;A PHP extension stopped the file upload.&apos;

);



// Outside a loop...

if($_FILES[&apos;userfile&apos;][&apos;error&apos;]==0) {

&#xA0; // process

} else {

&#xA0; $error_message = $error_types[$_FILES[&apos;userfile&apos;][&apos;error&apos;]];

&#xA0; // do whatever with the error message

}



// In a loop...

for($x=0,$y=count($_FILES[&apos;userfile&apos;][&apos;error&apos;]);$x&lt;$y;++$x) {

&#xA0; if($_FILES[&apos;userfile&apos;][&apos;error&apos;][$x]==0) {

&#xA0; &#xA0; // process

&#xA0; } else {

&#xA0; &#xA0; $error_message = $error_types[$_FILES[&apos;userfile&apos;][&apos;error&apos;][$x]];

&#xA0; &#xA0; // Do whatever with the error message

&#xA0; }

}



// When you&apos;re done... if you aren&apos;t doing all of this in a function that&apos;s about to end / complete all the processing and want to reclaim the memory

unset($error_types);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.errors.php)

**[To root](/README.md)**