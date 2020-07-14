# mysqli_result::fetch_field



here are the data types that correspond to the TYPE number returned by fetch_field. <br><br>thought i would post this here since i couldn&apos;t find the info elsewhere.<br><br>numerics <br>-------------<br>BIT: 16<br>TINYINT: 1<br>BOOL: 1<br>SMALLINT: 2<br>MEDIUMINT: 9<br>INTEGER: 3<br>BIGINT: 8<br>SERIAL: 8<br>FLOAT: 4<br>DOUBLE: 5<br>DECIMAL: 246<br>NUMERIC: 246<br>FIXED: 246<br><br>dates<br>------------<br>DATE: 10<br>DATETIME: 12<br>TIMESTAMP: 7<br>TIME: 11<br>YEAR: 13<br><br>strings &amp; binary<br>------------<br>CHAR: 254<br>VARCHAR: 253<br>ENUM: 254<br>SET: 254<br>BINARY: 254<br>VARBINARY: 253<br>TINYBLOB: 252<br>BLOB: 252<br>MEDIUMBLOB: 252<br>TINYTEXT: 252<br>TEXT: 252<br>MEDIUMTEXT: 252<br>LONGTEXT: 252  

#

The flags used by MySql are:                                                                                                                                            <br>       NOT_NULL_FLAG = 1                                                                              <br>       PRI_KEY_FLAG = 2                                                                               <br>       UNIQUE_KEY_FLAG = 4                                                                            <br>       BLOB_FLAG = 16                                                                                 <br>       UNSIGNED_FLAG = 32                                                                             <br>       ZEROFILL_FLAG = 64                                                                             <br>       BINARY_FLAG = 128                                                                              <br>       ENUM_FLAG = 256                                                                                <br>       AUTO_INCREMENT_FLAG = 512                                                                      <br>       TIMESTAMP_FLAG = 1024                                                                          <br>       SET_FLAG = 2048                                                                                <br>       NUM_FLAG = 32768                                                                               <br>       PART_KEY_FLAG = 16384                                                                          <br>       GROUP_FLAG = 32768                                                                             <br>       UNIQUE_FLAG = 65536                                                                            <br><br>To test if a flag is set you can use &amp; like so:<br>

```
<?php
$meta = $mysqli_result_object-&gt;fetch_field();
if ($meta-&gt;flags &amp; 4) { 
  echo &apos;Unique key flag is set&apos;; 
} 
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-field.php)

**[To root](/README.md)**