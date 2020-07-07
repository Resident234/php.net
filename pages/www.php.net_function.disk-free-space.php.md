# disk_free_space





Transformation is possible WITHOUT using loops:



```
<?php 
&#xA0; &#xA0; $bytes = disk_free_space(&quot;.&quot;); 
&#xA0; &#xA0; $si_prefix = array( &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; );
&#xA0; &#xA0; $base = 1024;
&#xA0; &#xA0; $class = min((int)log($bytes , $base) , count($si_prefix) - 1);
&#xA0; &#xA0; echo $bytes . &apos;&lt;br /&gt;&apos;;
&#xA0; &#xA0; echo sprintf(&apos;%1.2f&apos; , $bytes / pow($base,$class)) . &apos; &apos; . $si_prefix[$class] . &apos;&lt;br /&gt;&apos;;
?>
```



  

#



Nice, but please be aware of the prefixes.

SI specifies a lower case &apos;k&apos; as 1&apos;000 prefix.
It doesn&apos;t make sense to use an upper case &apos;K&apos; as binary prefix,
while the decimal Mega (M and following) prefixes in SI are uppercase.
Furthermore, there are REAL binary prefixes since a few years.

Do it the (newest and recommended) &quot;IEC&quot; way:

KB&apos;s are calculated decimal; power of 10 (1000 bytes each)
KiB&apos;s are calculated binary; power of 2 (1024 bytes each).
The same goes for MB, MiB and so on...

Feel free to read:
http://en.wikipedia.org/wiki/Binary_prefix

  

#



$si_prefix = array( &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; );

you are missing the petabyte after terabyte

 &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; 

should look like

 &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;PB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos;

  

#

[Official documentation page](https://www.php.net/manual/en/function.disk-free-space.php)

**[To root](/README.md)**