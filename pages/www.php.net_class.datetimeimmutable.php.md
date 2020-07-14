# The DateTimeImmutable class



Note: If you are trying to get 02.29 for non leap year it will return 03.01<br>Example: <br>

```
<?php 
new DateTimeImmutable(&apos;2017-02-29&apos;) // 2017-03-01
?>
```
  

#

Here&apos;s a simple example on how this class works :<br><br>    // Create a DateTimeImmutable Object<br>    $date = new DateTimeImmutable(&apos;2000-01-01&apos;);<br>    // "Change" that object and assign it&apos;s value to a new variable<br>    $date2 = $date-&gt;add(new DateInterval(&apos;P6M5DT24H&apos;));<br>    // Check out the content of the two variables<br>    echo $date-&gt;format(&apos;Y-m-d&apos;) . "\n";<br>    // 2000-01-01<br>    echo $date2-&gt;format(&apos;Y-m-d&apos;) . "\n";<br>    // 2000-07-07  

#

[Official documentation page](https://www.php.net/manual/en/class.datetimeimmutable.php)

**[To root](/README.md)**