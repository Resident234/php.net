# move_uploaded_file



Security tips you must know before use this function :<br><br>First : make sure that the file is not empty.<br><br>Second : make sure the file name in English characters, numbers and (_-.) symbols, For more protection.<br><br>You can use below function as in example<br><br>

```
<?php

/**
 * Check $_FILES[][name]
 *
 * @param (string) $filename - Uploaded file name.
 * @author Yousef Ismaeil Cliprz
 */
function check_file_uploaded_name ($filename)
{
    (bool) ((preg_match("`^[-0-9A-Z_\.]+Security tips you must know before use this function :<br><br>First : make sure that the file is not empty.<br><br>Second : make sure the file name in English characters, numbers and (_-.) symbols, For more protection.<br><br>You can use below function as in example<br><br>i",$filename)) ? true : false);
}

?>
```


Third : make sure that the file name not bigger than 250 characters.

as in example :



```
<?php

/**
 * Check $_FILES[][name] length.
 *
 * @param (string) $filename - Uploaded file name.
 * @author Yousef Ismaeil Cliprz.
 */
function check_file_uploaded_length ($filename)
{
    return (bool) ((mb_strlen($filename,"UTF-8") > 225) ? true : false);
}

?>
```


Fourth: Check File extensions and Mime Types that you want to allow in your project. You can use : pathinfo() http://php.net/pathinfo

or you can use regular expression for check File extensions as in example

#^(gif|jpg|jpeg|jpe|png)$#i

or use in_array checking as



```
<?php

$ext_type = array('gif','jpg','jpe','jpeg','png');

?>
```
<br><br>You have multi choices to checking extensions and Mime types.<br><br>Fifth: Check file size and make sure the limit of php.ini to upload files is what you want, You can start from http://www.php.net/manual/en/ini.core.php#ini.file-uploads<br><br>And last but not least : Check the file content if have a bad codes or something like this function http://php.net/manual/en/function.file-get-contents.php.<br><br>You can use .htaccess to stop working some scripts as in example php file in your upload path.<br><br>use :<br><br>AddHandler cgi-script .php .pl .jsp .asp .sh .cgi<br>Options -ExecCGI  <br><br>Do not forget this steps for your project protection.  

#

The destination directory must exist; move_uploaded_file() will not automatically create it for you.  

#

For those using PHP on Windows and IIS, you SHOULD set the "upload_tmp_dir" value in php.ini to some directory around where your websites directory is, create that directory, and then set the same permissions on it that you have set for your websites directory. Otherwise, when you upload a file and it goes into C:\WINDOWS\Temp, then you move it to your website directory, its permissions will NOT be set correctly. This will cause you problems if you then want to manipulate that file with something like ImageMagick&apos;s convert utility.  

#

[Official documentation page](https://www.php.net/manual/en/function.move-uploaded-file.php)

**[To root](/README.md)**