# mysqli_stmt::fetch



I was trying to use a generic select * from table statment and have the results returned in an array. I finally came up with this solution, others have similar solutions, but they where not working for me. <br>

```
<?php
    //Snip use normal methods to get to this point
    $stmt->execute();
    $metaResults = $stmt->result_metadata();
    $fields = $metaResults->fetch_fields();
    $statementParams='';
     //build the bind_results statement dynamically so I can get the results in an array
     foreach($fields as $field){
         if(empty($statementParams)){
             $statementParams.="\$column['".$field->name."']";
         }else{
             $statementParams.=", \$column['".$field->name."']";
         }
    }
    $statment="\$stmt->bind_result($statementParams);";
    eval($statment);
    while($stmt->fetch()){
        //Now the data is contained in the assoc array $column. Useful if you need to do a foreach, or 
        //if your lazy and didn't want to write out each param to bind.
    }
    // Continue on as usual.
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.fetch.php)

**[To root](/README.md)**