# The DateTimeImmutable class





Note: If you are trying to get 02.29 for non leap year it will return 03.01
Example: 


```
<?php 
new DateTimeImmutable(&apos;2017-02-29&apos;) // 2017-03-01
?>
```



  

#



Here&apos;s a simple example on how this class works :

&#xA0; &#xA0; // Create a DateTimeImmutable Object
&#xA0; &#xA0; $date = new DateTimeImmutable(&apos;2000-01-01&apos;);
&#xA0; &#xA0; // &quot;Change&quot; that object and assign it&apos;s value to a new variable
&#xA0; &#xA0; $date2 = $date-&gt;add(new DateInterval(&apos;P6M5DT24H&apos;));
&#xA0; &#xA0; // Check out the content of the two variables
&#xA0; &#xA0; echo $date-&gt;format(&apos;Y-m-d&apos;) . &quot;\n&quot;;
&#xA0; &#xA0; // 2000-01-01
&#xA0; &#xA0; echo $date2-&gt;format(&apos;Y-m-d&apos;) . &quot;\n&quot;;
&#xA0; &#xA0; // 2000-07-07

  

#

[Official documentation page](https://www.php.net/manual/en/class.datetimeimmutable.php)

**[To root](/README.md)**