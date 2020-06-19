# openssl_pkey_get_private




<div class="phpcode"><span class="html">
It&apos;s actually &quot;file://key.pem&quot; when you want to give a relative path using unix systems. It will be three &apos;/&apos; in case of absolute path (e.g &quot;file:///home/username/...&quot;). But this path consists of two &apos;/&apos; originated from &quot;file://&quot; and one &apos;/&apos; from the fact that home is a subfolder of the unix filesystem&apos;s root directory (&quot;/home/username/...&quot;). This two part will be concatenated and you will get three &apos;/&apos; characters following each other.<br><br>So you only have to concatenate &quot;file://&quot; with an existing path string in every case.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-pkey-get-private.php)

**[To root](/README.md)**