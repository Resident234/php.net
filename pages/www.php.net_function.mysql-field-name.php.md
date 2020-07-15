# mysql_field_name



This function is slightly stupid to be honest, why not just make an array of field names... You could consolidate the two of these functions that way and it makes it a lot easier to list them when your script is dynamic.<br><br>

```
<?php

    function mysql_field_array( $query ) {
    
        $field = mysql_num_fields( $query );
    
        for ( $i = 0; $i &lt; $field; $i++ ) {
        
            $names[] = mysql_field_name( $query, $i );
        
        }
        
        return $names;
    
    }
    
    // Examples of use
    
    $fields = mysql_field_array( $query );
    
    // Show name of column 3
    
    echo $fields[3];
    
    // Show them all
    
    echo implode( ', ', $fields[3] );
    
     // Count them - easy equivelant to 'mysql_num_fields'
    
    echo count( $fields );

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-field-name.php)

**[To root](/README.md)**