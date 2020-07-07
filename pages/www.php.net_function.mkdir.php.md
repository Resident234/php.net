# mkdir





When using the recursive parameter bear in mind that if you&apos;re using chmod() after mkdir() to set the mode without it being modified by the value of uchar() you need to call chmod() on all created directories. ie:



```
<?php
mkdir(&apos;/test1/test2&apos;, 0777, true);
chmod(&apos;/test1/test2&apos;, 0777);
?>
```
 

May result in &quot;/test1/test2&quot; having a mode of 0777 but &quot;/test1&quot; still having a mode of 0755 from the mkdir() call. You&apos;d need to do:



```
<?php
mkdir(&apos;/test1/test2&apos;, 0777, true);
chmod(&apos;/test1&apos;, 0777);
chmod(&apos;/test1/test2&apos;, 0777);
?>
```



  

#



This is an annotation from Stig Bakken:



The mode on your directory is affected by your current umask.&#xA0; It will end

up having (&lt;mkdir-mode&gt; and (not &lt;umask&gt;)).&#xA0; If you want to create one

that is publicly readable, do something like this:





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