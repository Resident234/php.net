# Error Messages Explained



Update to Adams old comment.<br><br>This is probably useful to someone.<br><br>

```
<?php<br><br>$phpFileUploadErrors = array(<br>    0 =&gt; &apos;There is no error, the file uploaded with success&apos;,<br>    1 =&gt; &apos;The uploaded file exceeds the upload_max_filesize directive in php.ini&apos;,<br>    2 =&gt; &apos;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&apos;,<br>    3 =&gt; &apos;The uploaded file was only partially uploaded&apos;,<br>    4 =&gt; &apos;No file was uploaded&apos;,<br>    6 =&gt; &apos;Missing a temporary folder&apos;,<br>    7 =&gt; &apos;Failed to write file to disk.&apos;,<br>    8 =&gt; &apos;A PHP extension stopped the file upload.&apos;,<br>);  

#

[EDIT BY danbrown AT php DOT net: This code is a fixed version of a note originally submitted by (Thalent, Michiel Thalen) on 04-Mar-2009.]<br><br><br>This is a handy exception to use when handling upload errors:<br><br>

```
<?php

class UploadException extends Exception
{
    public function __construct($code) {
        $message = $this-&gt;codeToMessage($code);
        parent::__construct($message, $code);
    }

    private function codeToMessage($code)
    {
        switch ($code) {
            case UPLOAD_ERR_INI_SIZE:
                $message = "The uploaded file exceeds the upload_max_filesize directive in php.ini";
                break;
            case UPLOAD_ERR_FORM_SIZE:
                $message = "The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form";
                break;
            case UPLOAD_ERR_PARTIAL:
                $message = "The uploaded file was only partially uploaded";
                break;
            case UPLOAD_ERR_NO_FILE:
                $message = "No file was uploaded";
                break;
            case UPLOAD_ERR_NO_TMP_DIR:
                $message = "Missing a temporary folder";
                break;
            case UPLOAD_ERR_CANT_WRITE:
                $message = "Failed to write file to disk";
                break;
            case UPLOAD_ERR_EXTENSION:
                $message = "File upload stopped by extension";
                break;

            default:
                $message = "Unknown upload error";
                break;
        }
        return $message;
    }
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

This is probably useful to someone.<br><br>

```
<?php
array(
        0=&gt;"There is no error, the file uploaded with success", 
        1=&gt;"The uploaded file exceeds the upload_max_filesize directive in php.ini", 
        2=&gt;"The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form"
        3=&gt;"The uploaded file was only partially uploaded",
        4=&gt;"No file was uploaded",
        6=&gt;"Missing a temporary folder" 
);
?>
```
  

#

if post is greater than post_max_size set in php.ini<br><br>$_FILES and $_POST will return empty  

#

One thing that is annoying is that the way these constant values are handled requires processing no error with the equality, which wastes a little bit of space.  Even though "no error" is 0, which typically evaluates to "false" in an if statement, it will always evaluate to true in this context.<br><br>So, instead of this:<br>-----<br>

```
<?php
if($_FILES[&apos;userfile&apos;][&apos;error&apos;]) {
  // handle the error
} else {
  // process
}
?>
```

-----
You have to do this:
-----


```
<?php
if($_FILES[&apos;userfile&apos;][&apos;error&apos;]==0) {
  // process
} else {
  // handle the error
}
?>
```

-----
Also, ctype_digit fails, but is_int works.  If you&apos;re wondering... no, it doesn&apos;t make any sense.

To Schoschie:

You ask the question:  Why make stuff complicated when you can make it easy?  I ask the same question since the version of the code you / Anonymous / Thalent (per danbrown) have posted is unnecessary overhead and would result in a function call, as well as a potentially lengthy switch statement.  In a loop, that would be deadly... try this instead:

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
  // process
} else {
  $error_message = $error_types[$_FILES[&apos;userfile&apos;][&apos;error&apos;]];
  // do whatever with the error message
}

// In a loop...
for($x=0,$y=count($_FILES[&apos;userfile&apos;][&apos;error&apos;]);$x&lt;$y;++$x) {
  if($_FILES[&apos;userfile&apos;][&apos;error&apos;][$x]==0) {
    // process
  } else {
    $error_message = $error_types[$_FILES[&apos;userfile&apos;][&apos;error&apos;][$x]];
    // Do whatever with the error message
  }
}

// When you&apos;re done... if you aren&apos;t doing all of this in a function that&apos;s about to end / complete all the processing and want to reclaim the memory
unset($error_types);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.errors.php)

**[To root](/README.md)**