# mysqli_stmt::fetch





I was trying to use a generic select * from table statment and have the results returned in an array. I finally came up with this solution, others have similar solutions, but they where not working for me. 


```
<?php
&#xA0; &#xA0; //Snip use normal methods to get to this point
&#xA0; &#xA0; $stmt-&gt;execute();
&#xA0; &#xA0; $metaResults = $stmt-&gt;result_metadata();
&#xA0; &#xA0; $fields = $metaResults-&gt;fetch_fields();
&#xA0; &#xA0; $statementParams=&apos;&apos;;
&#xA0; &#xA0;&#xA0; //build the bind_results statement dynamically so I can get the results in an array
&#xA0; &#xA0;&#xA0; foreach($fields as $field){
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if(empty($statementParams)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $statementParams.=&quot;\$column[&apos;&quot;.$field-&gt;name.&quot;&apos;]&quot;;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }else{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $statementParams.=&quot;, \$column[&apos;&quot;.$field-&gt;name.&quot;&apos;]&quot;;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; $statment=&quot;\$stmt-&gt;bind_result($statementParams);&quot;;
&#xA0; &#xA0; eval($statment);
&#xA0; &#xA0; while($stmt-&gt;fetch()){
&#xA0; &#xA0; &#xA0; &#xA0; //Now the data is contained in the assoc array $column. Useful if you need to do a foreach, or 
&#xA0; &#xA0; &#xA0; &#xA0; //if your lazy and didn&apos;t want to write out each param to bind.
&#xA0; &#xA0; }
&#xA0; &#xA0; // Continue on as usual.
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.fetch.php)

**[To root](/README.md)**