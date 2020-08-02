# $HTTP_RAW_POST_DATA



To get the Raw Post Data:<br><br>

```
<?php $postdata = file_get_contents("php://input"); ?>
```
<br><br>Please see the notes here:<br>http://us.php.net/manual/en/wrappers.php.php  

---

what is exaclty raw POST data?<br><br>Answer:<br><br>$_POST can be said as and outcome after splitting the $HTTP_RAW_POST_DATA, php splits the raw post data and formats in the way we see it in the $_POST For example:<br><br>    $HTTP_RAW_POST_DATA looks something like this<br><br>key1=value1&amp;key2=value2<br><br>    then $_POST would look like this:<br><br>$_POST = array(<br>    "key1" =&gt; "value1",<br>    "key2" =&gt; "value2",);  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.httprawpostdata.php)

**[To root](/README.md)**