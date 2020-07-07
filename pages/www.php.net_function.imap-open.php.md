# imap_open





One of the issues with gmail IMAP SSL authentication is related to Google&apos;s account security.

Once you get the login error once, sign out of all your google accounts. Then, visit this link:
http://www.google.com/accounts/DisplayUnlockCaptcha

Log in with the account you&apos;re attempting to access via imap.

Follow the steps and you&apos;ll then be able to login in to gmail with php imap.

It&apos;s visually shown here:
http://jeffreifman.com/filtered-open-source-imap-mail-filtering-software-for-php/configuring-gmail/

  

#



Using: 



```
<?php

imap_open( &quot;{server.example.com:143}INBOX&quot; , &apos;login&apos; , &apos;password&apos; );

?>
```




Got this error:

&quot;Couldn&apos;t open stream {server.example.com:143}INBOX&quot; 



Solved by adding the flag &quot;novalidate-cert&quot;:



```
<?php

imap_open( &quot;{server.example.com:143/novalidate-cert}INBOX&quot; , &apos;login&apos; , &apos;password&apos; );

?>
```




=D

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-open.php)

**[To root](/README.md)**