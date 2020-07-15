# microtime



You can use one variable to check execution $time as follow:<br><br>

```
<?php
$time = -microtime(true);
$hash = 0;
for ($i=0; $i < rand(1000,4000); ++$i) {
    $hash ^= md5(substr(str_shuffle("0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"), 0, rand(1,10)));
}
$time += microtime(true);
echo "Hash: $hash iterations:$i time: ",sprintf('%f', $time),PHP_EOL;
?>
```
  

#

Note that the timestamp returned is "with microseconds", not "in microseconds". This is especially good to know if you pass &apos;true&apos; as the parameter and then calculate the difference between two float values -- the result is already in seconds; it doesn&apos;t need to be divided by a million.  

#

All these timing scripts rely on microtime which relies on gettimebyday(2)<br><br>This can be inaccurate on servers that run ntp to syncronise the servers<br>time.<br><br>For timing, you should really use clock_gettime(2) with the<br>CLOCK_MONOTONIC flag set.<br><br>This returns REAL WORLD time, not affected by intentional clock drift.<br><br>This may seem a bit picky, but I recently saw a server that&apos;s clock was an<br>hour out, and they&apos;d set it to &apos;drift&apos; to the correct time (clock is speeded<br>up until it reaches the correct time)<br><br>Those sorts of things can make a real impact.<br><br>Any solutions, seeing as php doesn&apos;t have a hook into clock_gettime?<br><br>More info here: http://tinyurl.com/28vxja9<br><br>http://blog.habets.pp.se/2010/09/<br>gettimeofday-should-never-be-used-to-measure-time  

#

A lot of the comments here suggest adding in the following way:  (float)$usec + (float)$sec<br>Make sure you have the float precision high enough as with the default precision of 12, you are only precise to the 0.01 seconds.  <br>Set this in you php.ini file.<br>        precision    =  16  

#

[Official documentation page](https://www.php.net/manual/en/function.microtime.php)

**[To root](/README.md)**