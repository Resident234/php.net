# Session Upload Progress



Note, this feature doesn&apos;t work, when your webserver is runnig PHP via FastCGI. There will be no progress informations in the session array.<br>Unfortunately PHP gets the data only after the upload is completed and can&apos;t show any progress.<br><br>I hope this informations helps.  

#

ATTENTION:<br><br>Put the upload progress session name input field BEFORE your file field in the form :<br><br>  &lt;form action="upload.php" method="POST" enctype="multipart/form-data"&gt;<br>  &lt;input type="hidden" name="

```
<?php echo ini_get("session.upload_progress.name"); ?>
```
" value="123" /&gt;
  &lt;input type="file" name="file1" /&gt;
  &lt;input type="file" name="file2" /&gt;
  &lt;input type="submit" /&gt;
  &lt;/form&gt;

If you make it after your file field, you&apos;ll waste a lot of time figuring why (just like me ...)

The following form will make you crazy and waste really a lot of time:

&lt;form action="upload.php" method="POST" enctype="multipart/form-data"&gt;
 &lt;input type="file" name="file1" /&gt;
 &lt;input type="file" name="file2" /&gt;
 &lt;input type="hidden" name="

```
<?php echo ini_get("session.upload_progress.name"); ?>
```
" value="123" /&gt;<br> &lt;input type="submit" /&gt;<br>&lt;/form&gt;<br><br>DON&apos;T do this!  

#

While the example in the documentation is accurate, the description is a bit off. To clarify:<br><br>PHP will populate an array in the $_SESSION, where the index is a concatenated value of the session.upload_progress.prefix and the VALUE of the POSTed session.upload_progress.name variable.  

#

If you&apos;re seeing<br>"PHP Warning:  Unknown: The session id is too long or contains illegal characters, valid characters are a-z, A-Z, 0-9 and &apos;-,&apos; in Unknown on line 0",<br>then a misplaced input could be the cause. It&apos;s worth mentioning again that the hidden element MUST be before the file elements.  

#

[Official documentation page](https://www.php.net/manual/en/session.upload-progress.php)

**[To root](/README.md)**