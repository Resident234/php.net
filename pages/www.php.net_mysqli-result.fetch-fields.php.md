# mysqli_result::fetch_fields



The field info bit-flags used by MySql are:                                                                                                                                            <br> (Thanks to ragtag at hotmail dot com)<br>

```
<?php
/*
       NOT_NULL_FLAG = 1                                                                              
       PRI_KEY_FLAG = 2                                                                               
       UNIQUE_KEY_FLAG = 4                                                                            
       BLOB_FLAG = 16                                                                                 
       UNSIGNED_FLAG = 32                                                                             
       ZEROFILL_FLAG = 64                                                                             
       BINARY_FLAG = 128                                                                              
       ENUM_FLAG = 256                                                                                
       AUTO_INCREMENT_FLAG = 512                                                                      
       TIMESTAMP_FLAG = 1024                                                                          
       SET_FLAG = 2048                                                                                
       NUM_FLAG = 32768                                                                               
       PART_KEY_FLAG = 16384                                                                          
       GROUP_FLAG = 32768                                                                             
       UNIQUE_FLAG = 65536
*/                                                                            

// To test if a flag is set you can use &amp; like so:

  $meta = $mysqli_result_object->fetch_field();
  if ($meta->flags &amp; 4) { 
     echo 'Unique key flag is set'; 
  } 
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-fields.php)

**[To root](/README.md)**