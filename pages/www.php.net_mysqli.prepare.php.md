# mysqli::prepare



Just wanted to make sure that all were aware of get_result.<br><br>In the code sample, after execute(), perform a get_result() like this:<br><br>

```
<?php

// ... document's example code:

    /* bind parameters for markers */
    $stmt->bind_param("s", $city);

    /* execute query */
    $stmt->execute();

    /* instead of bind_result: */
    $result = $stmt->get_result();

    /* now you can fetch the results into an array - NICE */
    while ($myrow = $result->fetch_assoc()) {

        // use your $myrow array as you would with any other fetch
        printf("%s is in district %s\n", $city, $myrow['district']);

    }
?>
```
<br><br>This is much nicer when you have a dozen or more fields coming back from your query.  Hope this helps.  

---

I wrote this function for my personal use and figured I would share it.  I am not sure if this is the appropriate forum but I wish I had this when I stumbled on to mysqli::prepare.  The function is an update of the function I posted previously.  The previous function could not handle multiple queries.<br><br>For queries:<br>Results of single queries are given as arrays[row#][associated Data Array]<br>Results of multiple queries are given as arrays[query#][row#][associated Data Array]<br><br>For queries which return an affected row#, affected rows are returned instead of (array[row#][associated Data Array])<br><br>Code and example are below:<br><br>

```
<?php
function mysqli_prepared_query($link,$sql,$typeDef = FALSE,$params = FALSE){
  if($stmt = mysqli_prepare($link,$sql)){
    if(count($params) == count($params,1)){
      $params = array($params);
      $multiQuery = FALSE;
    } else {
      $multiQuery = TRUE;
    }  
    
    if($typeDef){
      $bindParams = array();    
      $bindParamsReferences = array();
      $bindParams = array_pad($bindParams,(count($params,1)-count($params))/count($params),"");         
      foreach($bindParams as $key => $value){
        $bindParamsReferences[$key] = &amp;$bindParams[$key];  
      }
      array_unshift($bindParamsReferences,$typeDef);
      $bindParamsMethod = new ReflectionMethod('mysqli_stmt', 'bind_param');
      $bindParamsMethod->invokeArgs($stmt,$bindParamsReferences);
    }
    
    $result = array();
    foreach($params as $queryKey => $query){
      foreach($bindParams as $paramKey => $value){
        $bindParams[$paramKey] = $query[$paramKey];
      }
      $queryResult = array();
      if(mysqli_stmt_execute($stmt)){
        $resultMetaData = mysqli_stmt_result_metadata($stmt);
        if($resultMetaData){                                                                               
          $stmtRow = array();   
          $rowReferences = array(); 
          while ($field = mysqli_fetch_field($resultMetaData)) { 
            $rowReferences[] = &amp;$stmtRow[$field->name]; 
          }                                
          mysqli_free_result($resultMetaData);
          $bindResultMethod = new ReflectionMethod('mysqli_stmt', 'bind_result'); 
          $bindResultMethod->invokeArgs($stmt, $rowReferences);
          while(mysqli_stmt_fetch($stmt)){
            $row = array();
            foreach($stmtRow as $key => $value){
              $row[$key] = $value;           
            }
            $queryResult[] = $row;
          }
          mysqli_stmt_free_result($stmt);
        } else {
          $queryResult[] = mysqli_stmt_affected_rows($stmt);
        }
      } else {
        $queryResult[] = FALSE;
      } 
      $result[$queryKey] = $queryResult;
    }
    mysqli_stmt_close($stmt);   
  } else {
    $result = FALSE;
  }
  
  if($multiQuery){
    return $result;
  } else {
    return $result[0];
  }
}
?>
```


Example(s):
For a table of firstName and lastName:
John Smith
Mark Smith
Jack Johnson
Bob Johnson



```
<?php
//single query, single result
$query = "SELECT * FROM names WHERE firstName=? AND lastName=?";
$params = array("Bob","Johnson");

mysqli_prepared_query($link,$query,"ss",$params)
/*
returns array(
0=> array('firstName' => 'Bob', 'lastName' => 'Johnson')
)
*/

//single query, multiple results
$query = "SELECT * FROM names WHERE lastName=?";
$params = array("Smith");

mysqli_prepared_query($link,$query,"s",$params)
/*
returns array(
0=> array('firstName' => 'John', 'lastName' => 'Smith')
1=> array('firstName' => 'Mark', 'lastName' => 'Smith')
)
*/

//multiple query, multiple results
$query = "SELECT * FROM names WHERE lastName=?";
$params = array(array("Smith"),array("Johnson"));

mysqli_prepared_query($link,$query,"s",$params)
/*
returns array(
0=>
array(
0=> array('firstName' => 'John', 'lastName' => 'Smith')
1=> array('firstName' => 'Mark', 'lastName' => 'Smith')
)
1=>
array(
0=> array('firstName' => 'Jack', 'lastName' => 'Johnson')
1=> array('firstName' => 'Bob', 'lastName' => 'Johnson')
)
)
*/
?>
```
<br><br>Hope it helps =)  

---

[Official documentation page](https://www.php.net/manual/en/mysqli.prepare.php)

**[To root](/README.md)**