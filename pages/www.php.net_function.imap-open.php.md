# imap_open



One of the issues with gmail IMAP SSL authentication is related to Google&apos;s account security.<br><br>Once you get the login error once, sign out of all your google accounts. Then, visit this link:<br>http://www.google.com/accounts/DisplayUnlockCaptcha<br><br>Log in with the account you&apos;re attempting to access via imap.<br><br>Follow the steps and you&apos;ll then be able to login in to gmail with php imap.<br><br>It&apos;s visually shown here:<br>http://jeffreifman.com/filtered-open-source-imap-mail-filtering-software-for-php/configuring-gmail/  

#

Using: <br>

```
<?php
imap_open( "{server.example.com:143}INBOX" , &apos;login&apos; , &apos;password&apos; );
?>
```


Got this error:
"Couldn&apos;t open stream {server.example.com:143}INBOX" 

Solved by adding the flag "novalidate-cert":


```
<?php
imap_open( "{server.example.com:143/novalidate-cert}INBOX" , &apos;login&apos; , &apos;password&apos; );
?>
```
<br><br>=D  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-open.php)

**[To root](/README.md)**