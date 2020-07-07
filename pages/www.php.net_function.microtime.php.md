# microtime





You can use one variable to check execution $time as follow:





```
<?php

$time = -microtime(true);

$hash = 0;

for ($i=0; $i &lt; rand(1000,4000); ++$i) {

&#xA0; &#xA0; $hash ^= md5(substr(str_shuffle(&quot;0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ&quot;), 0, rand(1,10)));

}

$time += microtime(true);

echo &quot;Hash: $hash iterations:$i time: &quot;,sprintf(&apos;%f&apos;, $time),PHP_EOL;

?>
```



  

#



Note that the timestamp returned is &quot;with microseconds&quot;, not &quot;in microseconds&quot;. This is especially good to know if you pass &apos;true&apos; as the parameter and then calculate the difference between two float values -- the result is already in seconds; it doesn&apos;t need to be divided by a million.

  

#



All these timing scripts rely on microtime which relies on gettimebyday(2)

This can be inaccurate on servers that run ntp to syncronise the servers
time.

For timing, you should really use clock_gettime(2) with the
CLOCK_MONOTONIC flag set.

This returns REAL WORLD time, not affected by intentional clock drift.

This may seem a bit picky, but I recently saw a server that&apos;s clock was an
hour out, and they&apos;d set it to &apos;drift&apos; to the correct time (clock is speeded
up until it reaches the correct time)

Those sorts of things can make a real impact.

Any solutions, seeing as php doesn&apos;t have a hook into clock_gettime?

More info here: http://tinyurl.com/28vxja9

http://blog.habets.pp.se/2010/09/
gettimeofday-should-never-be-used-to-measure-time

  

#



A lot of the comments here suggest adding in the following way:&#xA0; (float)$usec + (float)$sec
Make sure you have the float precision high enough as with the default precision of 12, you are only precise to the 0.01 seconds.&#xA0; 
Set this in you php.ini file.
&#xA0; &#xA0; &#xA0; &#xA0; precision&#xA0; &#xA0; =&#xA0; 16

  

#

[Official documentation page](https://www.php.net/manual/en/function.microtime.php)

**[To root](/README.md)**