# is_uploaded_file



Note that calling this function before move_uploaded_file() is not necessary, as it does the exact same checks already. It provides no extra security. Only when you&apos;re trying to use an uploaded file for something other than moving it to a new location.<br><br>Reference:<br>https://github.com/php/php-src/blob/master/ext/standard/basic_functions.c#L5796  

#

As of PHP 4.2.0, rather than automatically assuming a failed file uploaded is a file attack, you can use the error code associated with the file upload to check and see why the upload failed.  This error code is stored in the userfile array (ex: $HTTP_POST_FILES[&apos;userfile&apos;][&apos;error&apos;]). <br><br>Here&apos;s an example of a switch:<br><br>if (is_uploaded_file($userfile)) {<br>  <br>  //include code to copy tmp file to final location here...<br>  <br>}else{<br>  switch($HTTP_POST_FILES[&apos;userfile&apos;][&apos;error&apos;]){<br>    case 0: //no error; possible file attack!<br>      echo "There was a problem with your upload.";<br>      break;<br>    case 1: //uploaded file exceeds the upload_max_filesize directive in php.ini<br>      echo "The file you are trying to upload is too big.";<br>      break;<br>    case 2: //uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the html form<br>      echo "The file you are trying to upload is too big.";<br>      break;<br>    case 3: //uploaded file was only partially uploaded<br>      echo "The file you are trying upload was only partially uploaded.";<br>      break;<br>    case 4: //no file was uploaded<br>      echo "You must select an image for upload.";<br>      break;<br>    default: //a default error, just in case!  :)<br>      echo "There was a problem with your upload.";<br>      break;<br>}<br><br>Additionally, by testing the &apos;name&apos; element of the file upload array, you can filter out unwanted file types (.exe, .zip, .bat, etc).  Here&apos;s an example of a filter that can be added before testing to see if the file was uploaded:<br><br>//rejects all .exe, .com, .bat, .zip, .doc and .txt files<br>if(preg_match("/.exe$|.com$|.bat$|.zip$|.doc$|.txt$/i", $HTTP_POST_FILES[&apos;userfile&apos;][&apos;name&apos;])){<br>  exit("You cannot upload this type of file.");<br>}<br><br>//if file is not rejected by the filter, continue normally<br>if (is_uploaded_file($userfile)) {<br><br>/*rest of code*/  

#

[Official documentation page](https://www.php.net/manual/en/function.is-uploaded-file.php)

**[To root](/README.md)**