# touch





Note that when PHP is called by f.e. apache or nginx instead of directly from the command line, touch() will not prefix the location of the invoking script, so the supplied filename must contain an absolute path.

With script started from /home/user/www, this will not touch &quot;/home/user/www/somefile&quot;:



```
<?php
&#xA0; &#xA0; touch( &apos;somefile&apos; );
?>
```


But this will:



```
<?php
&#xA0; &#xA0; touch( __DIR__ . &apos;/somefile&apos; );
?>
```



  

#



I&apos;ve been trying to set a filemtime into the future with touch() on PHP5.

It seems touch $time has a future limit around 1000000 seconds (11 days or so). Beyond this point it reverts to a previous $time.

It doesn&apos;t make much sense but I could save you hours of time.

$time = time()+1500000;
touch($cachedfile,$time);

  

#

[Official documentation page](https://www.php.net/manual/en/function.touch.php)

**[To root](/README.md)**