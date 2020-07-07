# mail





Often it&apos;s helpful to find the exact error message that is triggered by the mail() function. While the function doesn&apos;t provide an error directly, you can use error_get_last() when mail() returns false.



```
<?php
$success = mail(&apos;example@example.com&apos;, &apos;My Subject&apos;, $message);
if (!$success) {
&#xA0; &#xA0; $errorMessage = error_get_last()[&apos;message&apos;];
}
?>
```


(Tested successfully on Windows which uses SMTP by default, but sendmail on Linux/OSX may not provide the same level of detail.)

Thanks to https://stackoverflow.com/a/20203870/195835

  

#



Security advice: Although it is not documented, for the parameters $to and $subject the mail() function changes at least \r and \n to space. So these parameters are safe against injection of additional headers. But you might want to check $to for commas as these separate multiple addresses and you might not want to send to more than one recipient.

The crucial part is the $additional_headers parameter. This parameter can&apos;t be cleaned by the mail() function. So it is up to you to prevent unwanted \r or \n to be inserted into the values you put in there. Otherwise you just created a potential spam distributor.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mail.php)

**[To root](/README.md)**