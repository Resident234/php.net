# max



The simplest way to get around the fact that max() won&apos;t give the key is array_search:<br><br>

```
<?php
$student_grades = array ("john" =&gt; 100, "sarah" =&gt; 90, "anne" =&gt; 100);
$top_student = array_search(max($student_grades),$student_grades); // john
?>
```


This could also be done with array_flip, though overwriting will mean that it gets the last max value rather than the first:



```
<?php
$grades_index = array_flip($student_grades);
$top_student = $grades_index[max($student_grades)]; // anne
?>
```


To get all the max value keys:



```
<?php
$top_students = array_keys($student_grades,max($student_grades)); // john, anne
?>
```
  

#

max(null, 0) = null<br>max(0, null) = 0  

#

max() (and min()) on DateTime objects compares them like dates (with timezone info) and returns DateTime object.<br>

```
<?php 
$dt1 = new DateTime(&apos;2014-05-07 18:53&apos;, new DateTimeZone(&apos;Europe/Kiev&apos;));
$dt2 = new DateTime(&apos;2014-05-07 16:53&apos;, new DateTimeZone(&apos;UTC&apos;));
echo max($dt1,$dt2)-&gt;format(DateTime::RFC3339) . PHP_EOL; // 2014-05-07T16:53:00+00:00
echo min($dt1,$dt2)-&gt;format(DateTime::RFC3339) . PHP_EOL; // 2014-05-07T18:53:00+03:00
?>
```
<br><br>It works at least 5.3.3-7+squeeze17  

#

Notice that whenever there is a Number in front of the String, it will be used for Comparison.<br><br>

```
<?php

  max(&apos;7iuwmssuxue&apos;, 1); //returns 7iuwmssuxu
  max(&apos;-7suidha&apos;, -4); //returns -4

?>
```


But just if it is in front of the String



```
<?php

  max(&apos;sdihatewin7wduiw&apos;, 3); //returns 3

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.max.php)

**[To root](/README.md)**