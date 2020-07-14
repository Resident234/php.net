# pcntl_signal



If you are using PHP &gt;= 7.1 then DO NOT use `declare(ticks=1)` instead use `pcntl_async_signals(true)`<br><br>There&apos;s no performance hit or overhead with `pcntl_async_signals()`. See this blog post https://blog.pascal-martin.fr/post/php71-en-other-new-things.html for a simple example of how to use this.  

#

Remember that signal handlers are called immediately, so every time when your process receives registered signal blocking functions like sleep() and usleep() are interrupted (or more like ended) like it was their time to finish.<br><br>This is expected behaviour, but not mentioned in this documentation. Sleep interruption can be very unfortunate when for example you run a daemon that is supposed to execute some important task exactly every X seconds/minutes - this task could be called too early or too often and it would be hard to debug why it&apos;s happening exactly.<br><br>Simply remember that signal handlers might interfere with other parts of your code.<br>Here is example workaround ensuring that function gets executed every minute no matter how many times our loop gets interrupted by incoming signal (SIGUSR1 in this case).<br><br>

```
<?php

  pcntl_async_signals(TRUE);

  pcntl_signal(SIGUSR1, function($signal) {
    // do something when signal is called
  });

  function everyMinute() {
    // do some important stuff
  }

  $wait = 60;
  $next = 0;

  while (TRUE) {
    $stamp = time();
    do {
      if ($stamp &gt;= $next) { break; }
      $diff = $next - $stamp;
      sleep($diff);
      $stamp = time();
    } while ($stamp &lt; $next);
    
    everyMinute();
    
    $next = $stamp + $wait;
    sleep($wait);
  }

?>
```
<br><br>So in this infinite loop do {} while() calculates and adds missing sleep() to ensure that our everyMinute() function is not called too early. Both sleep() functions here are covered so everyMinute() will never be executed before it&apos;s time even if process receives multiple SIGUSR1 signals during it&apos;s runtime.  

#

For PHP &gt;= 5.3.0, instead of declare(ticks = 1), you should now use pcntl_ signal_ dispatch().  

#

Remember that declaring a tick handler can become really expensive in terms of CPU cycles: Every n ticks the signal handling overhead will be executed. <br><br>So instead of declaring tick=1, test if tick=100 can do the job. If so, you are likely to gain fivefold speed.<br><br>As your script might always might miss some signals due to blocking operations like cURL downloads, call pcntl_signal_dispatch() on vital spots, e.g. at the beginning of your main loop.  

#

You cannot assign a signal handler for SIGKILL (kill -9).  

#

[Official documentation page](https://www.php.net/manual/en/function.pcntl-signal.php)

**[To root](/README.md)**