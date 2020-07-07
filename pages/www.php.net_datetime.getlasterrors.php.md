# DateTime::getLastErrors





DateTime::createFromFormat is smart to handle the cases where you input an invalid date, like April 31st, and convert it to May 1st. In some cases, you do not want this automatic smart handling of the dates for example in a user input form where you want to be sure that your user did input the date he wanted. To do that, you need to get access to the warnings, this method is the only way to do it:



```
<?php
$date = DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;1999-04-31&apos;);
print $date-&gt;format(&apos;Y-m-d&apos;) . PHP_EOL;
print_r(DateTime::getLastErrors());
?>
```


The output is:

1999-05-01
Array
(
&#xA0; &#xA0; [warning_count] =&gt; 1
&#xA0; &#xA0; [warnings] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [10] =&gt; The parsed date was invalid
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [error_count] =&gt; 0
&#xA0; &#xA0; [errors] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; )

)

So, here you can see, you have a warning because the date was invalid, but not an error because PHP was smart enough to convert it into a valid date. It is then up to you to do something with this information.

  

#

[Official documentation page](https://www.php.net/manual/en/datetime.getlasterrors.php)

**[To root](/README.md)**