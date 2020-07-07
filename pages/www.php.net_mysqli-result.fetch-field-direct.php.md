# mysqli_result::fetch_field_direct





Here&apos;s a bigger list of data types.&#xA0; I got this by creating every type I could and calling fetch_fields():



```
<?php
$mysql_data_type_hash = array(
&#xA0; &#xA0; 1=&gt;&apos;tinyint&apos;,
&#xA0; &#xA0; 2=&gt;&apos;smallint&apos;,
&#xA0; &#xA0; 3=&gt;&apos;int&apos;,
&#xA0; &#xA0; 4=&gt;&apos;float&apos;,
&#xA0; &#xA0; 5=&gt;&apos;double&apos;,
&#xA0; &#xA0; 7=&gt;&apos;timestamp&apos;,
&#xA0; &#xA0; 8=&gt;&apos;bigint&apos;,
&#xA0; &#xA0; 9=&gt;&apos;mediumint&apos;,
&#xA0; &#xA0; 10=&gt;&apos;date&apos;,
&#xA0; &#xA0; 11=&gt;&apos;time&apos;,
&#xA0; &#xA0; 12=&gt;&apos;datetime&apos;,
&#xA0; &#xA0; 13=&gt;&apos;year&apos;,
&#xA0; &#xA0; 16=&gt;&apos;bit&apos;,
&#xA0; &#xA0; //252 is currently mapped to all text and blob types (MySQL 5.0.51a)
&#xA0; &#xA0; 253=&gt;&apos;varchar&apos;,
&#xA0; &#xA0; 254=&gt;&apos;char&apos;,
&#xA0; &#xA0; 246=&gt;&apos;decimal&apos;
);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-field-direct.php)

**[To root](/README.md)**