# is_uploaded_file





Note that calling this function before move_uploaded_file() is not necessary, as it does the exact same checks already. It provides no extra security. Only when you&apos;re trying to use an uploaded file for something other than moving it to a new location.

Reference:
https://github.com/php/php-src/blob/master/ext/standard/basic_functions.c#L5796

  

#



As of PHP 4.2.0, rather than automatically assuming a failed file uploaded is a file attack, you can use the error code associated with the file upload to check and see why the upload failed.&#xA0; This error code is stored in the userfile array (ex: $HTTP_POST_FILES[&apos;userfile&apos;][&apos;error&apos;]). 

Here&apos;s an example of a switch:

if (is_uploaded_file($userfile)) {
&#xA0; 
&#xA0; //include code to copy tmp file to final location here...
&#xA0; 
}else{
&#xA0; switch($HTTP_POST_FILES[&apos;userfile&apos;][&apos;error&apos;]){
&#xA0; &#xA0; case 0: //no error; possible file attack!
&#xA0; &#xA0; &#xA0; echo &quot;There was a problem with your upload.&quot;;
&#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case 1: //uploaded file exceeds the upload_max_filesize directive in php.ini
&#xA0; &#xA0; &#xA0; echo &quot;The file you are trying to upload is too big.&quot;;
&#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case 2: //uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the html form
&#xA0; &#xA0; &#xA0; echo &quot;The file you are trying to upload is too big.&quot;;
&#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case 3: //uploaded file was only partially uploaded
&#xA0; &#xA0; &#xA0; echo &quot;The file you are trying upload was only partially uploaded.&quot;;
&#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case 4: //no file was uploaded
&#xA0; &#xA0; &#xA0; echo &quot;You must select an image for upload.&quot;;
&#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; default: //a default error, just in case!&#xA0; :)
&#xA0; &#xA0; &#xA0; echo &quot;There was a problem with your upload.&quot;;
&#xA0; &#xA0; &#xA0; break;
}

Additionally, by testing the &apos;name&apos; element of the file upload array, you can filter out unwanted file types (.exe, .zip, .bat, etc).&#xA0; Here&apos;s an example of a filter that can be added before testing to see if the file was uploaded:

//rejects all .exe, .com, .bat, .zip, .doc and .txt files
if(preg_match(&quot;/.exe$|.com$|.bat$|.zip$|.doc$|.txt$/i&quot;, $HTTP_POST_FILES[&apos;userfile&apos;][&apos;name&apos;])){
&#xA0; exit(&quot;You cannot upload this type of file.&quot;);
}

//if file is not rejected by the filter, continue normally
if (is_uploaded_file($userfile)) {

/*rest of code*/

  

#

[Official documentation page](https://www.php.net/manual/en/function.is-uploaded-file.php)

**[To root](/README.md)**