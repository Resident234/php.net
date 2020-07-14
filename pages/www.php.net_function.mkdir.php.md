# mkdir



When using the recursive parameter bear in mind that if you&apos;re using chmod() after mkdir() to set the mode without it being modified by the value of uchar() you need to call chmod() on all created directories. ie:<br><br>

```
<?php
mkdir(&apos;/test1/test2&apos;, 0777, true);
chmod(&apos;/test1/test2&apos;, 0777);
?>
```
 

May result in "/test1/test2" having a mode of 0777 but "/test1" still having a mode of 0755 from the mkdir() call. You&apos;d need to do:



```
<?php
mkdir(&apos;/test1/test2&apos;, 0777, true);
chmod(&apos;/test1&apos;, 0777);
chmod(&apos;/test1/test2&apos;, 0777);
?>
```
  

#

This is an annotation from Stig Bakken:<br><br>The mode on your directory is affected by your current umask.  It will end<br>up having (&lt;mkdir-mode&gt; and (not &lt;umask&gt;)).  If you want to create one<br>that is publicly readable, do something like this:<br><br>

```
<?php
$oldumask = umask(0);
mkdir(&apos;mydir&apos;, 0777); // or even 01777 so you get the sticky bit set
umask($oldumask);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.mkdir.php)

**[To root](/README.md)**