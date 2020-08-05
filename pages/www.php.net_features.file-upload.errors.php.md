# Error Messages Explained



Update to Adams old comment.<br><br>This is probably useful to someone.<br><br>

```
<?php

$phpFileUploadErrors = array(
    0 => 'There is no error, the file uploaded with success',
    1 => 'The uploaded file exceeds the upload_max_filesize directive in php.ini',
    2 => 'The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form',
    3 => 'The uploaded file was only partially uploaded',
    4 => 'No file was uploaded',
    6 => 'Missing a temporary folder',
    7 => 'Failed to write file to disk.',
    8 => 'A PHP extension stopped the file upload.',
);?>
```
  

---

[EDIT BY danbrown AT php DOT net: This code is a fixed version of a note originally submitted by (Thalent, Michiel Thalen) on 04-Mar-2009.]<br><br><br>This is a handy exception to use when handling upload errors:<br><br>

```
<?php

class UploadException extends Exception
{
    public function __construct($code) {
        $message = $this->codeToMessage($code);
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
 if ($_FILES['file']['error'] === UPLOAD_ERR_OK) {
//uploading successfully done
} else {
throw new UploadException($_FILES['file']['error']);
}
?>
```
  

---

This is probably useful to someone.<br><br>

```
<?php
array(
        0=>"There is no error, the file uploaded with success", 
        1=>"The uploaded file exceeds the upload_max_filesize directive in php.ini", 
        2=>"The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form"
        3=>"The uploaded file was only partially uploaded",
        4=>"No file was uploaded",
        6=>"Missing a temporary folder" 
);
?>
```
  

---

if post is greater than post_max_size set in php.ini<br><br>$_FILES and $_POST will return empty  

---

One thing that is annoying is that the way these constant values are handled requires processing no error with the equality, which wastes a little bit of space.  Even though "no error" is 0, which typically evaluates to "false" in an if statement, it will always evaluate to true in this context.<br><br>So, instead of this:<br>-----<br>

```
<?php
if($_FILES['userfile']['error']) {
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
if($_FILES['userfile']['error']==0) {
  // process
} else {
  // handle the error
}
?>
```

-----
Also, ctype_digit fails, but is_int works.  If you're wondering... no, it doesn't make any sense.

To Schoschie:

You ask the question:  Why make stuff complicated when you can make it easy?  I ask the same question since the version of the code you / Anonymous / Thalent (per danbrown) have posted is unnecessary overhead and would result in a function call, as well as a potentially lengthy switch statement.  In a loop, that would be deadly... try this instead:

-----


```
<?php
$error_types = array(
1=>'The uploaded file exceeds the upload_max_filesize directive in php.ini.',
'The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form.',
'The uploaded file was only partially uploaded.',
'No file was uploaded.',
6=>'Missing a temporary folder.',
'Failed to write file to disk.',
'A PHP extension stopped the file upload.'
);

// Outside a loop...
if($_FILES['userfile']['error']==0) {
  // process
} else {
  $error_message = $error_types[$_FILES['userfile']['error']];
  // do whatever with the error message
}

// In a loop...
for($x=0,$y=count($_FILES['userfile']['error']);$x<$y;++$x) {
  if($_FILES['userfile']['error'][$x]==0) {
    // process
  } else {
    $error_message = $error_types[$_FILES['userfile']['error'][$x]];
    // Do whatever with the error message
  }
}

// When you're done... if you aren't doing all of this in a function that's about to end / complete all the processing and want to reclaim the memory
unset($error_types);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/features.file-upload.errors.php)

**[To root](/README.md)**