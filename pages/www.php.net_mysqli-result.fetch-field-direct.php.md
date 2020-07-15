# mysqli_result::fetch_field_direct



Here&apos;s a bigger list of data types.  I got this by creating every type I could and calling fetch_fields():<br><br>

```
<?php
$mysql_data_type_hash = array(
    1=>'tinyint',
    2=>'smallint',
    3=>'int',
    4=>'float',
    5=>'double',
    7=>'timestamp',
    8=>'bigint',
    9=>'mediumint',
    10=>'date',
    11=>'time',
    12=>'datetime',
    13=>'year',
    16=>'bit',
    //252 is currently mapped to all text and blob types (MySQL 5.0.51a)
    253=>'varchar',
    254=>'char',
    246=>'decimal'
);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-field-direct.php)

**[To root](/README.md)**