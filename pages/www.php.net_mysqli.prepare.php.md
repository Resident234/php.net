# mysqli::prepare





Just wanted to make sure that all were aware of get_result.

In the code sample, after execute(), perform a get_result() like this:



```
<?php

// ... document&apos;s example code:

&#xA0; &#xA0; /* bind parameters for markers */
&#xA0; &#xA0; $stmt-&gt;bind_param(&quot;s&quot;, $city);

&#xA0; &#xA0; /* execute query */
&#xA0; &#xA0; $stmt-&gt;execute();

&#xA0; &#xA0; /* instead of bind_result: */
&#xA0; &#xA0; $result = $stmt-&gt;get_result();

&#xA0; &#xA0; /* now you can fetch the results into an array - NICE */
&#xA0; &#xA0; while ($myrow = $result-&gt;fetch_assoc()) {

&#xA0; &#xA0; &#xA0; &#xA0; // use your $myrow array as you would with any other fetch
&#xA0; &#xA0; &#xA0; &#xA0; printf(&quot;%s is in district %s\n&quot;, $city, $myrow[&apos;district&apos;]);

&#xA0; &#xA0; }
?>
```


This is much nicer when you have a dozen or more fields coming back from your query.&#xA0; Hope this helps.

  

#



I wrote this function for my personal use and figured I would share it.&#xA0; I am not sure if this is the appropriate forum but I wish I had this when I stumbled on to mysqli::prepare.&#xA0; The function is an update of the function I posted previously.&#xA0; The previous function could not handle multiple queries.



For queries:

Results of single queries are given as arrays[row#][associated Data Array]

Results of multiple queries are given as arrays[query#][row#][associated Data Array]



For queries which return an affected row#, affected rows are returned instead of (array[row#][associated Data Array])



Code and example are below:





```
<?php

function mysqli_prepared_query($link,$sql,$typeDef = FALSE,$params = FALSE){

&#xA0; if($stmt = mysqli_prepare($link,$sql)){

&#xA0; &#xA0; if(count($params) == count($params,1)){

&#xA0; &#xA0; &#xA0; $params = array($params);

&#xA0; &#xA0; &#xA0; $multiQuery = FALSE;

&#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; $multiQuery = TRUE;

&#xA0; &#xA0; }&#xA0; 

&#xA0; &#xA0; 

&#xA0; &#xA0; if($typeDef){

&#xA0; &#xA0; &#xA0; $bindParams = array();&#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; $bindParamsReferences = array();

&#xA0; &#xA0; &#xA0; $bindParams = array_pad($bindParams,(count($params,1)-count($params))/count($params),&quot;&quot;);&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; foreach($bindParams as $key =&gt; $value){

&#xA0; &#xA0; &#xA0; &#xA0; $bindParamsReferences[$key] = &amp;$bindParams[$key];&#xA0; 

&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; array_unshift($bindParamsReferences,$typeDef);

&#xA0; &#xA0; &#xA0; $bindParamsMethod = new ReflectionMethod(&apos;mysqli_stmt&apos;, &apos;bind_param&apos;);

&#xA0; &#xA0; &#xA0; $bindParamsMethod-&gt;invokeArgs($stmt,$bindParamsReferences);

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; $result = array();

&#xA0; &#xA0; foreach($params as $queryKey =&gt; $query){

&#xA0; &#xA0; &#xA0; foreach($bindParams as $paramKey =&gt; $value){

&#xA0; &#xA0; &#xA0; &#xA0; $bindParams[$paramKey] = $query[$paramKey];

&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; $queryResult = array();

&#xA0; &#xA0; &#xA0; if(mysqli_stmt_execute($stmt)){

&#xA0; &#xA0; &#xA0; &#xA0; $resultMetaData = mysqli_stmt_result_metadata($stmt);

&#xA0; &#xA0; &#xA0; &#xA0; if($resultMetaData){&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmtRow = array();&#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rowReferences = array(); 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while ($field = mysqli_fetch_field($resultMetaData)) { 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rowReferences[] = &amp;$stmtRow[$field-&gt;name]; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; mysqli_free_result($resultMetaData);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bindResultMethod = new ReflectionMethod(&apos;mysqli_stmt&apos;, &apos;bind_result&apos;); 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bindResultMethod-&gt;invokeArgs($stmt, $rowReferences);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while(mysqli_stmt_fetch($stmt)){

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $row = array();

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($stmtRow as $key =&gt; $value){

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $row[$key] = $value;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $queryResult[] = $row;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; mysqli_stmt_free_result($stmt);

&#xA0; &#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $queryResult[] = mysqli_stmt_affected_rows($stmt);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; $queryResult[] = FALSE;

&#xA0; &#xA0; &#xA0; } 

&#xA0; &#xA0; &#xA0; $result[$queryKey] = $queryResult;

&#xA0; &#xA0; }

&#xA0; &#xA0; mysqli_stmt_close($stmt);&#xA0;&#xA0; 

&#xA0; } else {

&#xA0; &#xA0; $result = FALSE;

&#xA0; }

&#xA0; 

&#xA0; if($multiQuery){

&#xA0; &#xA0; return $result;

&#xA0; } else {

&#xA0; &#xA0; return $result[0];

&#xA0; }

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

$query = &quot;SELECT * FROM names WHERE firstName=? AND lastName=?&quot;;

$params = array(&quot;Bob&quot;,&quot;Johnson&quot;);



mysqli_prepared_query($link,$query,&quot;ss&quot;,$params)

/*

returns array(

0=&gt; array(&apos;firstName&apos; =&gt; &apos;Bob&apos;, &apos;lastName&apos; =&gt; &apos;Johnson&apos;)

)

*/



//single query, multiple results

$query = &quot;SELECT * FROM names WHERE lastName=?&quot;;

$params = array(&quot;Smith&quot;);



mysqli_prepared_query($link,$query,&quot;s&quot;,$params)

/*

returns array(

0=&gt; array(&apos;firstName&apos; =&gt; &apos;John&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)

1=&gt; array(&apos;firstName&apos; =&gt; &apos;Mark&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)

)

*/



//multiple query, multiple results

$query = &quot;SELECT * FROM names WHERE lastName=?&quot;;

$params = array(array(&quot;Smith&quot;),array(&quot;Johnson&quot;));



mysqli_prepared_query($link,$query,&quot;s&quot;,$params)

/*

returns array(

0=&gt;

array(

0=&gt; array(&apos;firstName&apos; =&gt; &apos;John&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)

1=&gt; array(&apos;firstName&apos; =&gt; &apos;Mark&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)

)

1=&gt;

array(

0=&gt; array(&apos;firstName&apos; =&gt; &apos;Jack&apos;, &apos;lastName&apos; =&gt; &apos;Johnson&apos;)

1=&gt; array(&apos;firstName&apos; =&gt; &apos;Bob&apos;, &apos;lastName&apos; =&gt; &apos;Johnson&apos;)

)

)

*/

?>
```




Hope it helps =)

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.prepare.php)

**[To root](/README.md)**