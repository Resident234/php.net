# Connection handling



hey, thanks to arr1, and it is very useful for me, when I need to return to the user fast and then do something else.<br><br>When using the codes, it nearly drive me mad and I found another thing that may affect the codes:<br><br>Content-Encoding: gzip<br><br>This is because the zlib is on and the content will be compressed. But this will not output the buffer until all output is over.<br><br>So, it may need to send the header to prevent this problem.<br><br>now, the code becomes:<br><br>

```
<?php
ob_end_clean();
header("Connection: close\r\n");
header("Content-Encoding: none\r\n");
ignore_user_abort(true); // optional
ob_start();
echo (&apos;Text user will see&apos;);
$size = ob_get_length();
header("Content-Length: $size");
ob_end_flush();     // Strange behaviour, will not work
flush();            // Unless both are called !
ob_end_clean();

//do processing here
sleep(5);

echo(&apos;Text user will never see&apos;);
//do some processing
?>
```
  

#

Closing the users browser connection whilst keeping your php script running has been an issue since 4.1, when the behaviour of register_shutdown_function() was modified so that it would not automatically close the users connection.<br><br>sts at mail dot xubion dot hu<br>Posted the original solution:<br><br>

```
<?php
header("Connection: close");
ob_start();
phpinfo();
$size=ob_get_length();
header("Content-Length: $size");
ob_end_flush();
flush();
sleep(13);
error_log("do something in the background");
?>
```


Which works fine until you substitute phpinfo() for 
echo (&apos;text I want user to see&apos;); in which case the headers are never sent!

The solution is to explicitly turn off output buffering and clear the buffer prior to sending your header information.

example:



```
<?php
 ob_end_clean();
 header("Connection: close");
 ignore_user_abort(); // optional
 ob_start();
 echo (&apos;Text the user will see&apos;);
 $size = ob_get_length();
 header("Content-Length: $size");
 ob_end_flush(); // Strange behaviour, will not work
 flush();            // Unless both are called !
 // Do processing here 
 sleep(30);
 echo(&apos;Text user will never see&apos;);
?>
```
<br> <br>Just spent 3 hours trying to figure this one out, hope it helps someone :)<br><br>Tested in:<br>IE 7.5730.11<br>Mozilla Firefox 1.81  

#

I had a lot of problems getting a redirect to work, after which my script was intended to keep working in the background. The redirect to another page of my site simply would only work once the original page had finished processing.<br><br>I finally found out what was wrong:<br>The session only gets closed by PHP at the very end of the script, and since access to the session data is locked to prevent more than one page writing to it simultaneously, the new page cannot load until the original processing has finished.<br><br>Solution:<br>Close the session manually when redirecting using session_write_close():<br><br>

```
<?php
ignore_user_abort(true);
set_time_limit(0);

$strURL = "PUT YOUR REDIRCT HERE";
header("Location: $strURL", true);
header("Connection: close", true);
header("Content-Encoding: none\r\n");
header("Content-Length: 0", true);

flush();
ob_flush();

session_write_close();

// Continue processing...

sleep(100);
exit;
?>
```
<br><br>But careful:<br>Make sure that your script doesn&apos;t write to the session after session_write_close(), i.e. in your background processing code.  That won&apos;t work.  Also avoid reading, remember, the next script may already have modified the data.<br><br>So try to read out the data you need prior to redirecting.  

#

PHP changes directory on connection abort so code like this will not do what you want:<br><br>

```
<?php
function abort()
{
     if(connection_aborted())
           unlink(&apos;file.ini&apos;);
}
register_shutdown_function(&apos;abort&apos;);
?>
```


actually it will delete file in apaches&apos;s root dir so if you want to unlink file in your script&apos;s dir on abort or write to it you have to store directory


```
<?php
function abort()
{
     global $dsd;
     if(connection_aborted())
           unlink($dsd.&apos;/file.ini&apos;);
}
register_shutdown_function(&apos;abort&apos;);
$dsd=getcwd();
?>
```
  

#

The point mentioned in the last comment isn&apos;t always the case.<br><br>If a user&apos;s connection is lost half way through an order processing script is confirming a user&apos;s credit card/adding them to a DB, etc (due to their ISP going down, network trouble... whatever) and your script tries to send back output (such as, "pre-processing order" or any other type of confirmation), then your script will abort -- and this could cause problems for your process.<br><br>I have an order script that adds data to a InnoDB database (through MySQL) and only commits the transactions upon successful completion. Without ignore_user_abort(), I have had times when a user&apos;s connection dropped during the processing phase... and their card was charged, but they weren&apos;t added to my local DB.<br><br>So, it&apos;s always safe to ignore any aborts if you are processing sensitive transactions that should go ahead, whether your user is "watching" on the other end or not.  

#

[Official documentation page](https://www.php.net/manual/en/features.connection-handling.php)

**[To root](/README.md)**