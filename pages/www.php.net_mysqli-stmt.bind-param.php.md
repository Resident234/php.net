# mysqli_stmt::bind_param





There are some things to note when working with mysqli::bind_param() and array-elements.

Re-assigning an array will break any references, no matter if the keys are identical.

You have to explicitly reassign every single value in an array, for the references to be kept.

Best shown in an example:



```
<?php

function getData() {

&#xA0; &#xA0; return array(

&#xA0; &#xA0; &#xA0; &#xA0; 0=&gt;array(

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;name&quot;=&gt;&quot;test_0&quot;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;email&quot;=&gt;&quot;test_0@example.com&quot;

&#xA0; &#xA0; &#xA0; &#xA0; ),

&#xA0; &#xA0; &#xA0; &#xA0; 1=&gt;array(

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;name&quot;=&gt;&quot;test_1&quot;,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;email&quot;=&gt;&quot;test_1@example.com&quot;

&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; );

}

$db&#xA0; = new mysqli(&quot;localhost&quot;,&quot;root&quot;,&quot;&quot;,&quot;tests&quot;);

$sql = &quot;INSERT INTO `user` SET `name`=?,`email`=?&quot;;

$res = $db-&gt;prepare($sql);

// If you bind array-elements to a prepared statement, the array has to be declared first with the used keys:

$arr = array(&quot;name&quot;=&gt;&quot;&quot;,&quot;email&quot;=&gt;&quot;&quot;); 

$res-&gt;bind_param(&quot;ss&quot;,$arr[&apos;name&apos;],$arr[&apos;email&apos;]);

//So far the introduction...



/* 

&#xA0; &#xA0; Example 1 (wont work as expected, creates two empty entries)

&#xA0; &#xA0; Re-assigning the array in the while()-head generates a new array, whereas references from bind_param stick to the old array

*/

foreach( getData() as $arr ) {

&#xA0; &#xA0; $res-&gt;execute();

}



/*

&#xA0; &#xA0; Example 2 (will work as expected)

&#xA0; &#xA0; Re-assigning every single value explicitly keeps the references alive

*/

foreach( getData() as $tempArr ) {

&#xA0; &#xA0; foreach($tempArr as $k=&gt;$v) {

&#xA0; &#xA0; &#xA0; &#xA0; $arr[$k] = $v;

&#xA0; &#xA0; }

&#xA0; &#xA0; $res-&gt;execute();

}

?>
```




Coming to the problem calling mysqli::bind_param() with a dynamic number of arguments via call_user_func_array() with PHP Version 5.3+, there&apos;s another workaround besides using an extra function to build the references for the array-elements.

You can use Reflection to call mysqli::bind_param(). When using PHP 5.3+ this saves you about 20-40% Speed compared to passing the array to your own reference-builder-function.

Example:



```
<?php

$db&#xA0; &#xA0;&#xA0; = new mysqli(&quot;localhost&quot;,&quot;root&quot;,&quot;&quot;,&quot;tests&quot;); 

$res&#xA0; &#xA0; = $db-&gt;prepare(&quot;INSERT INTO test SET foo=?,bar=?&quot;); 

$refArr = array(&quot;si&quot;,&quot;hello&quot;,42); 

$ref&#xA0; &#xA0; = new ReflectionClass(&apos;mysqli_stmt&apos;); 

$method = $ref-&gt;getMethod(&quot;bind_param&quot;); 

$method-&gt;invokeArgs($res,$refArr); 

$res-&gt;execute();&#xA0; 

?>
```



  

#



A few notes on this function.

If you specify type &quot;i&quot; (integer), the maximum value it allows you to have is 2^32-1 or 2147483647. So, if you are using UNSIGNED INTEGER or BIGINT in your database, then you are better off using &quot;s&quot; (string) for this.

Here&apos;s a quick summary:
(UN)SIGNED TINYINT: I
(UN)SIGNED SMALLINT: I
(UN)SIGNED MEDIUMINT: I
SIGNED INT: I
UNSIGNED INT: S
(UN)SIGNED BIGINT: S

(VAR)CHAR, (TINY/SMALL/MEDIUM/BIG)TEXT/BLOB should all have S.

FLOAT/REAL/DOUBLE (PRECISION) should all be D.

That advice was for MySQL. I have not looked into other database software.

  

#



Hi, I just write a function to do all my sql statements based on all the others comments in this page, maybe it can be useful for someone else :)

Usage:

execSQL($sql, $parameters, $close);

$sql = Statement to execute;
$parameters = array of type and values of the parameters (if any)
$close = true to close $stmt (in inserts) false to return an array with the values;

Examples:

execSQL(&quot;SELECT * FROM table WHERE id = ?&quot;, array(&apos;i&apos;, $id), false);

execSQL(&quot;SELECT * FROM table&quot;, array(), false);

execSQL(&quot;INSERT INTO table(id, name) VALUES (?,?)&quot;, array(&apos;ss&apos;, $id, $name), true);



```
<?php

function execSQL($sql, $params, $close){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $mysqli = new mysqli(&quot;localhost&quot;, &quot;user&quot;, &quot;pass&quot;, &quot;db&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $stmt = $mysqli-&gt;prepare($sql) or die (&quot;Failed to prepared the statement!&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; call_user_func_array(array($stmt, &apos;bind_param&apos;), refValues($params));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $stmt-&gt;execute();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if($close){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $result = $mysqli-&gt;affected_rows;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $meta = $stmt-&gt;result_metadata();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; while ( $field = $meta-&gt;fetch_field() ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $parameters[] = &amp;$row[$field-&gt;name];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; call_user_func_array(array($stmt, &apos;bind_result&apos;), refValues($parameters));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while ( $stmt-&gt;fetch() ) {&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $x = array();&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; foreach( $row as $key =&gt; $val ) {&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $x[$key] = $val;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $results[] = $x;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = $results;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $stmt-&gt;close();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $mysqli-&gt;close();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return&#xA0; $result;
&#xA0;&#xA0; }
&#xA0;&#xA0; 
&#xA0; &#xA0; function refValues($arr){
&#xA0; &#xA0; &#xA0; &#xA0; if (strnatcmp(phpversion(),&apos;5.3&apos;) &gt;= 0) //Reference is required for PHP 5.3+
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $refs = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($arr as $key =&gt; $value)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $refs[$key] = &amp;$arr[$key];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $refs;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $arr;
&#xA0; &#xA0; }
?>
```


Regards

  

#



Blob and null handling aside, a couple of notes on how param values are automatically converted and forwarded on to the Mysql engine based on your type string argument:

1) PHP will automatically convert the value behind the scenes to the underlying type corresponding to your binding type string.&#xA0; i.e.:



```
<?php

$var = true;
bind_param(&apos;i&apos;, $var); // forwarded to Mysql as 1

?>
```


2) Though PHP numbers cannot be reliably cast to (int) if larger than PHP_INT_MAX, behind the scenes, the value will be converted anyway to at most long long depending on the size.&#xA0; This means that keeping in mind precision limits and avoiding manually casting the variable to (int) first, you can still use the &apos;i&apos; binding type for larger numbers.&#xA0; i.e.:



```
<?php

$var = &apos;429496729479896&apos;;
bind_param(&apos;i&apos;, $var); // forwarded to Mysql as 429496729479900

?>
```


3) You can default to &apos;s&apos; for most parameter arguments in most cases.&#xA0; The value will then be automatically cast to string on the back-end before being passed to the Mysql engine.&#xA0; Mysql will then perform its own conversions with values it receives from PHP on execute.&#xA0; This allows you to bind not only to larger numbers without concern for precision, but also to objects as long as that object has a &apos;__toString&apos; method.

This auto-string casting behavior greatly improves things like datetime handling.&#xA0; For example: if you extended DateTime class to add a __toString method which outputs the datetime format expected by Mysql, you can just bind to that DateTime_Extended object using type &apos;s&apos;.&#xA0; i.e.:



```
<?php

// DateTime_Extended has __toString defined to return the Mysql formatted datetime
$var = new DateTime_Extended;
bind_param(&apos;s&apos;, $var); // forwarded to Mysql as &apos;2011-03-14 17:00:01&apos;

?>
```



  

#



Dear all,



I was searching for a class which supports multiple calls to bind_param, because I have scenarios where I build huge SQL statements over different functions with variable numbers of parameters. But I didn&apos;t found one. So I have just written up this little piece of code I would like to share with you. There is enough room to optimize these classes, but it shows the general idea. And for me it works. In mbind_param_do() it seems to depend from the PHP version if makeValuesReferenced() must be used or if $params can be used directly. In my case I have to use it.



The cool thing about this solution: You don&apos;t have to care about a lot if you are using my mbind_ functions or not. You may also use default bind_param and the execute will still work. 





```
<?php



class db extends mysqli {

&#xA0; &#xA0; public function prepare($query) {

&#xA0; &#xA0; &#xA0; &#xA0; return new stmt($this,$query);

&#xA0; &#xA0; }

}



class stmt extends mysqli_stmt {

&#xA0; &#xA0; public function __construct($link, $query) {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_reset();

&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct($link, $query);

&#xA0; &#xA0; }



&#xA0; &#xA0; public function mbind_reset() {

&#xA0; &#xA0; &#xA0; &#xA0; unset($this-&gt;mbind_params);

&#xA0; &#xA0; &#xA0; &#xA0; unset($this-&gt;mbind_types);

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_params = array();

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_types = array();

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; //use this one to bind params by reference

&#xA0; &#xA0; public function mbind_param($type, &amp;$param) {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_types[0].= $type;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_params[] = &amp;$param;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; //use this one to bin value directly, can be mixed with mbind_param()

&#xA0; &#xA0; public function mbind_value($type, $param) {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_types[0].= $type;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_params[] = $param;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; 

&#xA0; &#xA0; public function mbind_param_do() {

&#xA0; &#xA0; &#xA0; &#xA0; $params = array_merge($this-&gt;mbind_types, $this-&gt;mbind_params);

&#xA0; &#xA0; &#xA0; &#xA0; return call_user_func_array(array($this, &apos;bind_param&apos;), $this-&gt;makeValuesReferenced($params));

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; private function makeValuesReferenced($arr){

&#xA0; &#xA0; &#xA0; &#xA0; $refs = array();

&#xA0; &#xA0; &#xA0; &#xA0; foreach($arr as $key =&gt; $value)

&#xA0; &#xA0; &#xA0; &#xA0; $refs[$key] = &amp;$arr[$key];

&#xA0; &#xA0; &#xA0; &#xA0; return $refs;



&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; public function execute() {

&#xA0; &#xA0; &#xA0; &#xA0; if(count($this-&gt;mbind_params))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;mbind_param_do();

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; return parent::execute();

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; private $mbind_types = array();

&#xA0; &#xA0; private $mbind_params = array();

}



$search1 = &quot;test1&quot;;

$search2 = &quot;test2&quot;;



$_db = new db(&quot;host&quot;,&quot;user&quot;,&quot;pass&quot;,&quot;database&quot;);

$query = &quot;SELECT name FROM table WHERE col1=? AND col2=?&quot;;

$stmt = $_db-&gt;prepare($query);



$stmt-&gt;mbind_param(&apos;s&apos;,$search1);

//this second call is the cool thing!!!

$stmt-&gt;mbind_param(&apos;s&apos;,$search2);



$stmt-&gt;execute();



//this would still work!

//$search1 = &quot;test1changed&quot;;

//$search2 = &quot;test2changed&quot;;

//$stmt-&gt;execute();



...



$stmt-&gt;store_result();

$stmt-&gt;bind_result(...);

$stmt-&gt;fetch();

?>
```



  

#



I had a problem with the LIKE operator



This code did not work:





```
<?php

$test = $sql-&gt;prepare(&quot;SELECT name FROM names WHERE name LIKE %?%&quot;);

$test-&gt;bind_param(&quot;s&quot;, $myname);

?>
```




The solution is:





```
<?php

$test = $sql-&gt;prepare(&quot;SELECT name FROM names WHERE name LIKE ?&quot;);

$param = &quot;%&quot; . $myname . &quot;%&quot;;

$test-&gt;bind_param(&quot;s&quot;, $param);

?>
```



  

#



I used to have problems with call_user_func_array and bind_param after migrating to php 5.3.



The problem is that 5.3 requires array values as reference while 5.2 works with real values.



so i created a secondary function to help me with this...





```
<?php

function refValues($arr){

&#xA0; &#xA0; if (strnatcmp(phpversion(),&apos;5.3&apos;) &gt;= 0) //Reference is required for PHP 5.3+

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $refs = array();

&#xA0; &#xA0; &#xA0; &#xA0; foreach($arr as $key =&gt; $value)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $refs[$key] = &amp;$arr[$key];

&#xA0; &#xA0; &#xA0; &#xA0; return $refs; 

&#xA0; &#xA0; }

&#xA0; &#xA0; return $arr;

}

?>
```




and changed my previous function from:





```
<?php

call_user_func_array(array($this-&gt;stmt, &quot;bind_param&quot;),$this-&gt;valores);

?>
```




to:





```
<?php

call_user_func_array(array($this-&gt;stmt, &quot;bind_param&quot;),refValues($this-&gt;valores));

?>
```




in this way my db functions keep working in php 5.2/5.3 servers.



I hope this help someone.

  

#



When dealing with a dynamic number of field values while preparing a statement I find this class useful.





```
<?php

class BindParam{

&#xA0; &#xA0; private $values = array(), $types = &apos;&apos;;

&#xA0; &#xA0; 

&#xA0; &#xA0; public function add( $type, &amp;$value ){

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;values[] = $value;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;types .= $type;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; public function get(){

&#xA0; &#xA0; &#xA0; &#xA0; return array_merge(array($this-&gt;types), $this-&gt;values);

&#xA0; &#xA0; }

}

?>
```




Usage is pretty simple. Create an instance and use the add method to populate. When you&apos;re ready to execute simply use the get method.





```
<?php

$bindParam = new BindParam();

$qArray = array();



$use_part_1 = 1;

$use_part_2 = 1;

$use_part_3 = 1;



$query = &apos;SELECT * FROM users WHERE &apos;;

if($use_part_1){

&#xA0; &#xA0; $qArray[] = &apos;hair_color = ?&apos;;

&#xA0; &#xA0; $bindParam-&gt;add(&apos;s&apos;, &apos;red&apos;);

}

if($use_part_2){

&#xA0; &#xA0; $qArray[] = &apos;age = ?&apos;;

&#xA0; &#xA0; $bindParam-&gt;add(&apos;i&apos;, 25);

}

if($use_part_3){

&#xA0; &#xA0; $qArray[] = &apos;balance = ?&apos;;

&#xA0; &#xA0; $bindParam-&gt;add(&apos;d&apos;, 50.00);

}



$query .= implode(&apos; OR &apos;, $qArray);



//call_user_func_array( array($stm, &apos;bind_param&apos;), $bindParam-&gt;get());



echo $query . &apos;&lt;br/&gt;&apos;;

var_dump($bindParam-&gt;get());

?>
```




This gets you the result that looks something like this:



SELECT * FROM users WHERE hair_color = ? OR age = ? OR balance = ?

array(4) { [0]=&gt; string(3) &quot;sid&quot; [1]=&gt; string(3) &quot;red&quot; [2]=&gt; int(25) [3]=&gt; float(50) }



[Editor&apos;s note: changed BindParam::add() to accept $value by reference and thereby prevent a warning in newer versions of PHP.]

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.bind-param.php)

**[To root](/README.md)**