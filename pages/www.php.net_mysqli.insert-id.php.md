# mysqli::$insert_id





I have received many statements that the insert_id property has a bug because it &quot;works sometimes&quot;.&#xA0; Keep in mind that when using the OOP approach, the actual instantiation of the mysqli class will hold the insert_id.&#xA0; 

The following code will return nothing.


```
<?php
$mysqli = new mysqli(&apos;host&apos;,&apos;user&apos;,&apos;pass&apos;,&apos;db&apos;);
if ($result = $mysqli-&gt;query(&quot;INSERT INTO t (field) VALUES (&apos;value&apos;);&quot;)) {
&#xA0;&#xA0; echo &apos;The ID is: &apos;.$result-&gt;insert_id;
}
?>
```


This is because the insert_id property doesn&apos;t belong to the result, but rather the actual mysqli class.&#xA0; This would work:



```
<?php
$mysqli = new mysqli(&apos;host&apos;,&apos;user&apos;,&apos;pass&apos;,&apos;db&apos;);
if ($result = $mysqli-&gt;query(&quot;INSERT INTO t (field) VALUES (&apos;value&apos;);&quot;)) {
&#xA0;&#xA0; echo &apos;The ID is: &apos;.$mysqli-&gt;insert_id;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.insert-id.php)

**[To root](/README.md)**