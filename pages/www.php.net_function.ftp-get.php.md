# ftp_get



I tried to ftp a 7mb file today off my webserver.<br><br>I copied this example directly and it told me.<br><br>Port command successful<br>"there was a problem"<br><br>I thought it was because of the size.<br>But I guessed it might be cause of my firewall.<br><br>So I made the ftp connection passive:<br><br>

```
<?php
  
  ...
  $login_result = ftp_login($conn_id, $ftp_user_name, $ftp_user_pass);
  ftp_pasv($conn_id, true);

?>
```
<br><br>Ran the script again &amp; it worked fine.<br><br>Hope this helps someone  

#

Don&apos;t want to use an intermediate file?  Use &apos;php://output&apos; as the filename and then capture the output using output buffering.<br><br>ob_start();<br>$result = ftp_get($ftp, "php://output", $file, FTP_BINARY);<br>$data = ob_get_contents();<br>ob_end_clean();<br><br>Don&apos;t forget to check $result to make sure there wasn&apos;t an error.  After that, manipulate the $data variable however you want.  

#

Keep in mind that ftp_get will overwrite the file on your local machine if it has the same name.  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-get.php)

**[To root](/README.md)**