# mysql_field_name





This function is slightly stupid to be honest, why not just make an array of field names... You could consolidate the two of these functions that way and it makes it a lot easier to list them when your script is dynamic.



```
<?php

&#xA0; &#xA0; function mysql_field_array( $query ) {
&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $field = mysql_num_fields( $query );
&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; for ( $i = 0; $i &lt; $field; $i++ ) {
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $names[] = mysql_field_name( $query, $i );
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; return $names;
&#xA0; &#xA0; 
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; // Examples of use
&#xA0; &#xA0; 
&#xA0; &#xA0; $fields = mysql_field_array( $query );
&#xA0; &#xA0; 
&#xA0; &#xA0; // Show name of column 3
&#xA0; &#xA0; 
&#xA0; &#xA0; echo $fields[3];
&#xA0; &#xA0; 
&#xA0; &#xA0; // Show them all
&#xA0; &#xA0; 
&#xA0; &#xA0; echo implode( &apos;, &apos;, $fields[3] );
&#xA0; &#xA0; 
&#xA0; &#xA0;&#xA0; // Count them - easy equivelant to &apos;mysql_num_fields&apos;
&#xA0; &#xA0; 
&#xA0; &#xA0; echo count( $fields );

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-field-name.php)

**[To root](/README.md)**