# sleep



This may seem obvious, but I thought I would save someone from something that just confused me: you cannot use sleep() to sleep for fractions of a second. This:<br><br>

```
<?php sleep(0.25) ?>
```


will not work as expected. The 0.25 is cast to an integer, so this is equivalent to sleep(0). To sleep for a quarter of a second, use:



```
<?php usleep(250000) ?>
```
  

---

re: "mitigating the chances of a full bruit force attack by a limit of 30 lookups a minute."<br><br>Not really - the attacker could do 100 requests. Each request might take 2 seconds but it doesn&apos;t stop the number of requests done. You need to stop processing more than one request every 2 seconds rather than delay it by 2 seconds on each execution.  

---

Maybe obvious, but this my function to delay script execution using decimals for seconds (to mimic sleep(1.5) for example):<br><br>

```
<?php
/**
 * Delays execution of the script by the given time.
 * @param mixed $time Time to pause script execution. Can be expressed
 * as an integer or a decimal.
 * @example msleep(1.5); // delay for 1.5 seconds
 * @example msleep(.1); // delay for 100 milliseconds
 */
function msleep($time)
{
    usleep($time * 1000000);
}
?>
```
  

---

You should put sleep into both the pass and fail branches, since an attacker can check whether the response is slow and use that as an indicator - cutting down the delay time. But a delay in both branches eliminates this possibility.  

---

Note: The set_time_limit() function and the configuration directive max_execution_time only affect the execution time of the script itself. Any time spent on activity that happens outside the execution of the script such as system calls using system(), the sleep() function, database queries, etc. is not included when determining the maximum time that the script has been running.  

---

it is a bad idea to use sleep() for delayed output effects as<br><br>1) you have to flush() output before you sleep<br><br>2) depending on your setup flush() will not work all the way to the browser as the web server might apply buffering of its own or the browser might not render output it thinks not to be complete<br><br>netscape for example will only display complete lines and will not show table parts until the &lt;/table&gt; tag arrived<br><br>so use sleep if you have to wait  for events and don&apos;t want to burn  to much cycles, but don&apos;t use it for silly delayed output effects!  

---

[Official documentation page](https://www.php.net/manual/en/function.sleep.php)

**[To root](/README.md)**