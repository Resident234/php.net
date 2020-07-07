# tmpfile





To get the underlying file path of a tmpfile file pointer:



```
<?php
$file = tmpfile();
$path = stream_get_meta_data($file)[&apos;uri&apos;]; // eg: /tmp/phpFx0513a


  

#



I found this function useful when uploading a file through FTP. One of the files I was uploading was input from a textarea on the previous page, so really there was no &quot;file&quot; to upload, this solved the problem nicely:



```
<?php
&#xA0; &#xA0; # Upload setup.inc
&#xA0; &#xA0; $fSetup = tmpfile();
&#xA0; &#xA0; fwrite($fSetup,$setup);
&#xA0; &#xA0; fseek($fSetup,0);
&#xA0; &#xA0; if (!ftp_fput($ftp,&quot;inc/setup.inc&quot;,$fSetup,FTP_ASCII)) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;br /&gt;&lt;i&gt;Setup file NOT inserted&lt;/i&gt;&lt;br /&gt;&lt;br /&gt;&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; fclose($fSetup);
?>
```


The $setup variable is the contents of the textarea.

And I&apos;m not sure if you need the fseek($temp,0); in there either, just leave it unless you know it doesn&apos;t effect it.

  

#

[Official documentation page](https://www.php.net/manual/en/function.tmpfile.php)

**[To root](/README.md)**