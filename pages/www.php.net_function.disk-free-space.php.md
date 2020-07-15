# disk_free_space



Transformation is possible WITHOUT using loops:<br><br>

```
<?php 
    $bytes = disk_free_space("."); 
    $si_prefix = array( 'B', 'KB', 'MB', 'GB', 'TB', 'EB', 'ZB', 'YB' );
    $base = 1024;
    $class = min((int)log($bytes , $base) , count($si_prefix) - 1);
    echo $bytes . '&lt;br /&gt;';
    echo sprintf('%1.2f' , $bytes / pow($base,$class)) . ' ' . $si_prefix[$class] . '&lt;br /&gt;';
?>
```
  

#

Nice, but please be aware of the prefixes.<br><br>SI specifies a lower case &apos;k&apos; as 1&apos;000 prefix.<br>It doesn&apos;t make sense to use an upper case &apos;K&apos; as binary prefix,<br>while the decimal Mega (M and following) prefixes in SI are uppercase.<br>Furthermore, there are REAL binary prefixes since a few years.<br><br>Do it the (newest and recommended) "IEC" way:<br><br>KB&apos;s are calculated decimal; power of 10 (1000 bytes each)<br>KiB&apos;s are calculated binary; power of 2 (1024 bytes each).<br>The same goes for MB, MiB and so on...<br><br>Feel free to read:<br>http://en.wikipedia.org/wiki/Binary_prefix  

#

$si_prefix = array( &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; );<br><br>you are missing the petabyte after terabyte<br><br> &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; <br><br>should look like<br><br> &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;PB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos;  

#

[Official documentation page](https://www.php.net/manual/en/function.disk-free-space.php)

**[To root](/README.md)**