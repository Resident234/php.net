# gnupg_encrypt



After spending some time trying to get this extension to work, I&apos;ve found that you have to have the GNUPGHOME environment variable set so that the keychain can be found, and have it set equal to the .gnupg directory itself, not the apache/httpd user&apos;s home directory (which is what is shown in dan&apos;s example code).  below is an example of this and a simple function I was working on at the time to encrypt a piece of data for storage in a database.<br><br>

```
<?php
    // set the environment so gnupg can find the keyring
    putenv("GNUPGHOME=/home/apache/.gnupg");

    function encrypt_string($str,$fingerprint) {
        $res = gnupg_init();
        gnupg_addencryptkey($res,$fingerprint);
        $enc = gnupg_encrypt($res, $str);
        return $enc;
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.gnupg-encrypt.php)

**[To root](/README.md)**