# max





The simplest way to get around the fact that max() won&apos;t give the key is array_search:



```
<?php
$student_grades = array (&quot;john&quot; =&gt; 100, &quot;sarah&quot; =&gt; 90, &quot;anne&quot; =&gt; 100);
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



max(null, 0) = null

max(0, null) = 0

  

#



max() (and min()) on DateTime objects compares them like dates (with timezone info) and returns DateTime object.


```
<?php 
$dt1 = new DateTime(&apos;2014-05-07 18:53&apos;, new DateTimeZone(&apos;Europe/Kiev&apos;));
$dt2 = new DateTime(&apos;2014-05-07 16:53&apos;, new DateTimeZone(&apos;UTC&apos;));
echo max($dt1,$dt2)-&gt;format(DateTime::RFC3339) . PHP_EOL; // 2014-05-07T16:53:00+00:00
echo min($dt1,$dt2)-&gt;format(DateTime::RFC3339) . PHP_EOL; // 2014-05-07T18:53:00+03:00
?>
```


It works at least 5.3.3-7+squeeze17

  

#



Notice that whenever there is a Number in front of the String, it will be used for Comparison.



```
<?php

&#xA0; max(&apos;7iuwmssuxue&apos;, 1); //returns 7iuwmssuxu
&#xA0; max(&apos;-7suidha&apos;, -4); //returns -4

?>
```


But just if it is in front of the String



```
<?php

&#xA0; max(&apos;sdihatewin7wduiw&apos;, 3); //returns 3

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.max.php)

**[To root](/README.md)**