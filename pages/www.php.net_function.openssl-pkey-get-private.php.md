# openssl_pkey_get_private



It&apos;s actually "file://key.pem" when you want to give a relative path using unix systems. It will be three &apos;/&apos; in case of absolute path (e.g "file:///home/username/..."). But this path consists of two &apos;/&apos; originated from "file://" and one &apos;/&apos; from the fact that home is a subfolder of the unix filesystem&apos;s root directory ("/home/username/..."). This two part will be concatenated and you will get three &apos;/&apos; characters following each other.<br><br>So you only have to concatenate "file://" with an existing path string in every case.  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-pkey-get-private.php)

**[To root](/README.md)**