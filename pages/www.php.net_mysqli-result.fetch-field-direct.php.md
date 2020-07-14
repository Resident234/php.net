# mysqli_result::fetch_field_direct



Here&apos;s a bigger list of data types.  I got this by creating every type I could and calling fetch_fields():<br><br>

```
<?php
$mysql_data_type_hash = array(
    1=&gt;&apos;tinyint&apos;,
    2=&gt;&apos;smallint&apos;,
    3=&gt;&apos;int&apos;,
    4=&gt;&apos;float&apos;,
    5=&gt;&apos;double&apos;,
    7=&gt;&apos;timestamp&apos;,
    8=&gt;&apos;bigint&apos;,
    9=&gt;&apos;mediumint&apos;,
    10=&gt;&apos;date&apos;,
    11=&gt;&apos;time&apos;,
    12=&gt;&apos;datetime&apos;,
    13=&gt;&apos;year&apos;,
    16=&gt;&apos;bit&apos;,
    //252 is currently mapped to all text and blob types (MySQL 5.0.51a)
    253=&gt;&apos;varchar&apos;,
    254=&gt;&apos;char&apos;,
    246=&gt;&apos;decimal&apos;
);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-field-direct.php)

**[To root](/README.md)**