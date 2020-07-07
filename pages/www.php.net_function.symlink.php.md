# symlink





Here is a simple way to control who downloads your files...



You will have to set: $filename, $downloaddir, $safedir and $downloadURL.



Basically $filename is the name of a file, $downloaddir is any dir on your server, $safedir is a dir that is not accessible by a browser that contains a file named $filename and $downloadURL is the URL equivalent of your $downloaddir.



The way this works is when a user wants to download a file, a randomly named dir is created in the $downloaddir, and a symbolic link is created to the file being requested.&#xA0; The browser is then redirected to the new link and the download begins.



The code also deletes any past symbolic links created by any past users before creating one for itself.&#xA0; This in effect leaves only one symbolic link at a time and prevents past users from downloading the file again without going through this script.&#xA0; There appears to be no problem if a symbolic link is deleted while another person is downloading from that link.



This is not too great if not many people download the file since the symbolic link will not be deleted until another person downloads the same file. 



Anyway enjoy:





```
<?php

$letters = &apos;abcdefghijklmnopqrstuvwxyz&apos;;

srand((double) microtime() * 1000000);

$string = &apos;&apos;;

for ($i = 1; $i &lt;= rand(4,12); $i++) {

&#xA0;&#xA0; $q = rand(1,24);

&#xA0;&#xA0; $string = $string . $letters[$q];

}

$handle = opendir($downloaddir);

while ($dir = readdir($handle)) {

&#xA0;&#xA0; if (is_dir($downloaddir . $dir)){

&#xA0; &#xA0; &#xA0; if ($dir != &quot;.&quot; &amp;&amp; $dir != &quot;..&quot;){

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; @unlink($downloaddir . $dir . &quot;/&quot; . $filename);

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; @rmdir($downloaddir . $dir);

&#xA0; &#xA0; &#xA0; }

&#xA0;&#xA0; }

}

closedir($handle);

mkdir($downloaddir . $string, 0777);

symlink($safedir . $filename, $downloaddir . $string . &quot;/&quot; . $filename);

Header(&quot;Location: &quot; . $downloadURL . $string . &quot;/&quot; . $filename);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.symlink.php)

**[To root](/README.md)**