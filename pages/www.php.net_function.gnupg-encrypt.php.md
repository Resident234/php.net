# gnupg_encrypt





After spending some time trying to get this extension to work, I&apos;ve found that you have to have the GNUPGHOME environment variable set so that the keychain can be found, and have it set equal to the .gnupg directory itself, not the apache/httpd user&apos;s home directory (which is what is shown in dan&apos;s example code).&#xA0; below is an example of this and a simple function I was working on at the time to encrypt a piece of data for storage in a database.



```
<?php
&#xA0; &#xA0; // set the environment so gnupg can find the keyring
&#xA0; &#xA0; putenv(&quot;GNUPGHOME=/home/apache/.gnupg&quot;);

&#xA0; &#xA0; function encrypt_string($str,$fingerprint) {
&#xA0; &#xA0; &#xA0; &#xA0; $res = gnupg_init();
&#xA0; &#xA0; &#xA0; &#xA0; gnupg_addencryptkey($res,$fingerprint);
&#xA0; &#xA0; &#xA0; &#xA0; $enc = gnupg_encrypt($res, $str);
&#xA0; &#xA0; &#xA0; &#xA0; return $enc;
&#xA0; &#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.gnupg-encrypt.php)

**[To root](/README.md)**