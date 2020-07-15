# mysqli_stmt::bind_param



There are some things to note when working with mysqli::bind_param() and array-elements.<br>Re-assigning an array will break any references, no matter if the keys are identical.<br>You have to explicitly reassign every single value in an array, for the references to be kept.<br>Best shown in an example:<br>

```
<?php
function getData() {
    return array(
        0=>array(
            "name"=>"test_0",
            "email"=>"test_0@example.com"
        ),
        1=>array(
            "name"=>"test_1",
            "email"=>"test_1@example.com"
        )
    );
}
$db  = new mysqli("localhost","root","","tests");
$sql = "INSERT INTO `user` SET `name`=?,`email`=?";
$res = $db->prepare($sql);
// If you bind array-elements to a prepared statement, the array has to be declared first with the used keys:
$arr = array("name"=>"","email"=>""); 
$res->bind_param("ss",$arr['name'],$arr['email']);
//So far the introduction...

/* 
    Example 1 (wont work as expected, creates two empty entries)
    Re-assigning the array in the while()-head generates a new array, whereas references from bind_param stick to the old array
*/
foreach( getData() as $arr ) {
    $res->execute();
}

/*
    Example 2 (will work as expected)
    Re-assigning every single value explicitly keeps the references alive
*/
foreach( getData() as $tempArr ) {
    foreach($tempArr as $k=>$v) {
        $arr[$k] = $v;
    }
    $res->execute();
}
?>
```


Coming to the problem calling mysqli::bind_param() with a dynamic number of arguments via call_user_func_array() with PHP Version 5.3+, there's another workaround besides using an extra function to build the references for the array-elements.
You can use Reflection to call mysqli::bind_param(). When using PHP 5.3+ this saves you about 20-40% Speed compared to passing the array to your own reference-builder-function.
Example:


```
<?php
$db     = new mysqli("localhost","root","","tests"); 
$res    = $db->prepare("INSERT INTO test SET foo=?,bar=?"); 
$refArr = array("si","hello",42); 
$ref    = new ReflectionClass('mysqli_stmt'); 
$method = $ref->getMethod("bind_param"); 
$method->invokeArgs($res,$refArr); 
$res->execute();  
?>
```
  

#

A few notes on this function.<br><br>If you specify type "i" (integer), the maximum value it allows you to have is 2^32-1 or 2147483647. So, if you are using UNSIGNED INTEGER or BIGINT in your database, then you are better off using "s" (string) for this.<br><br>Here&apos;s a quick summary:<br>(UN)SIGNED TINYINT: I<br>(UN)SIGNED SMALLINT: I<br>(UN)SIGNED MEDIUMINT: I<br>SIGNED INT: I<br>UNSIGNED INT: S<br>(UN)SIGNED BIGINT: S<br><br>(VAR)CHAR, (TINY/SMALL/MEDIUM/BIG)TEXT/BLOB should all have S.<br><br>FLOAT/REAL/DOUBLE (PRECISION) should all be D.<br><br>That advice was for MySQL. I have not looked into other database software.  

#

Hi, I just write a function to do all my sql statements based on all the others comments in this page, maybe it can be useful for someone else :)<br><br>Usage:<br><br>execSQL($sql, $parameters, $close);<br><br>$sql = Statement to execute;<br>$parameters = array of type and values of the parameters (if any)<br>$close = true to close $stmt (in inserts) false to return an array with the values;<br><br>Examples:<br><br>execSQL("SELECT * FROM table WHERE id = ?", array(&apos;i&apos;, $id), false);<br><br>execSQL("SELECT * FROM table", array(), false);<br><br>execSQL("INSERT INTO table(id, name) VALUES (?,?)", array(&apos;ss&apos;, $id, $name), true);<br><br>

```
<?php

function execSQL($sql, $params, $close){
           $mysqli = new mysqli("localhost", "user", "pass", "db");
           
           $stmt = $mysqli->prepare($sql) or die ("Failed to prepared the statement!");
           
           call_user_func_array(array($stmt, 'bind_param'), refValues($params));
           
           $stmt->execute();
           
           if($close){
               $result = $mysqli->affected_rows;
           } else {
               $meta = $stmt->result_metadata();
            
               while ( $field = $meta->fetch_field() ) {
                   $parameters[] = &amp;$row[$field->name];
               }  
        
            call_user_func_array(array($stmt, 'bind_result'), refValues($parameters));
               
            while ( $stmt->fetch() ) {  
               $x = array();  
               foreach( $row as $key => $val ) {  
                  $x[$key] = $val;  
               }  
               $results[] = $x;  
            }

            $result = $results;
           }
           
           $stmt->close();
           $mysqli->close();
           
           return  $result;
   }
   
    function refValues($arr){
        if (strnatcmp(phpversion(),'5.3') >= 0) //Reference is required for PHP 5.3+
        {
            $refs = array();
            foreach($arr as $key => $value)
                $refs[$key] = &amp;$arr[$key];
            return $refs;
        }
        return $arr;
    }
?>
```
<br><br>Regards  

#

Blob and null handling aside, a couple of notes on how param values are automatically converted and forwarded on to the Mysql engine based on your type string argument:<br><br>1) PHP will automatically convert the value behind the scenes to the underlying type corresponding to your binding type string.  i.e.:<br><br>

```
<?php

$var = true;
bind_param('i', $var); // forwarded to Mysql as 1

?>
```


2) Though PHP numbers cannot be reliably cast to (int) if larger than PHP_INT_MAX, behind the scenes, the value will be converted anyway to at most long long depending on the size.  This means that keeping in mind precision limits and avoiding manually casting the variable to (int) first, you can still use the 'i' binding type for larger numbers.  i.e.:



```
<?php

$var = '429496729479896';
bind_param('i', $var); // forwarded to Mysql as 429496729479900

?>
```


3) You can default to 's' for most parameter arguments in most cases.  The value will then be automatically cast to string on the back-end before being passed to the Mysql engine.  Mysql will then perform its own conversions with values it receives from PHP on execute.  This allows you to bind not only to larger numbers without concern for precision, but also to objects as long as that object has a '__toString' method.

This auto-string casting behavior greatly improves things like datetime handling.  For example: if you extended DateTime class to add a __toString method which outputs the datetime format expected by Mysql, you can just bind to that DateTime_Extended object using type 's'.  i.e.:



```
<?php

// DateTime_Extended has __toString defined to return the Mysql formatted datetime
$var = new DateTime_Extended;
bind_param('s', $var); // forwarded to Mysql as '2011-03-14 17:00:01'

?>
```
  

#

Dear all,<br><br>I was searching for a class which supports multiple calls to bind_param, because I have scenarios where I build huge SQL statements over different functions with variable numbers of parameters. But I didn&apos;t found one. So I have just written up this little piece of code I would like to share with you. There is enough room to optimize these classes, but it shows the general idea. And for me it works. In mbind_param_do() it seems to depend from the PHP version if makeValuesReferenced() must be used or if $params can be used directly. In my case I have to use it.<br><br>The cool thing about this solution: You don&apos;t have to care about a lot if you are using my mbind_ functions or not. You may also use default bind_param and the execute will still work. <br><br>

```
<?php

class db extends mysqli {
    public function prepare($query) {
        return new stmt($this,$query);
    }
}

class stmt extends mysqli_stmt {
    public function __construct($link, $query) {
        $this->mbind_reset();
        parent::__construct($link, $query);
    }

    public function mbind_reset() {
        unset($this->mbind_params);
        unset($this->mbind_types);
        $this->mbind_params = array();
        $this->mbind_types = array();
    }
    
    //use this one to bind params by reference
    public function mbind_param($type, &amp;$param) {
        $this->mbind_types[0].= $type;
        $this->mbind_params[] = &amp;$param;
    }
    
    //use this one to bin value directly, can be mixed with mbind_param()
    public function mbind_value($type, $param) {
        $this->mbind_types[0].= $type;
        $this->mbind_params[] = $param;
    }
    
    
    public function mbind_param_do() {
        $params = array_merge($this->mbind_types, $this->mbind_params);
        return call_user_func_array(array($this, 'bind_param'), $this->makeValuesReferenced($params));
    }
    
    private function makeValuesReferenced($arr){
        $refs = array();
        foreach($arr as $key => $value)
        $refs[$key] = &amp;$arr[$key];
        return $refs;

    }
    
    public function execute() {
        if(count($this->mbind_params))
            $this->mbind_param_do();
            
        return parent::execute();
    }
    
    private $mbind_types = array();
    private $mbind_params = array();
}

$search1 = "test1";
$search2 = "test2";

$_db = new db("host","user","pass","database");
$query = "SELECT name FROM table WHERE col1=? AND col2=?";
$stmt = $_db->prepare($query);

$stmt->mbind_param('s',$search1);
//this second call is the cool thing!!!
$stmt->mbind_param('s',$search2);

$stmt->execute();

//this would still work!
//$search1 = "test1changed";
//$search2 = "test2changed";
//$stmt->execute();

...

$stmt->store_result();
$stmt->bind_result(...);
$stmt->fetch();
?>
```
  

#

I had a problem with the LIKE operator<br><br>This code did not work:<br><br>

```
<?php
$test = $sql->prepare("SELECT name FROM names WHERE name LIKE %?%");
$test->bind_param("s", $myname);
?>
```


The solution is:



```
<?php
$test = $sql->prepare("SELECT name FROM names WHERE name LIKE ?");
$param = "%" . $myname . "%";
$test->bind_param("s", $param);
?>
```
  

#

I used to have problems with call_user_func_array and bind_param after migrating to php 5.3.<br><br>The problem is that 5.3 requires array values as reference while 5.2 works with real values.<br><br>so i created a secondary function to help me with this...<br><br>

```
<?php
function refValues($arr){
    if (strnatcmp(phpversion(),'5.3') >= 0) //Reference is required for PHP 5.3+
    {
        $refs = array();
        foreach($arr as $key => $value)
            $refs[$key] = &amp;$arr[$key];
        return $refs; 
    }
    return $arr;
}
?>
```


and changed my previous function from:



```
<?php
call_user_func_array(array($this->stmt, "bind_param"),$this->valores);
?>
```


to:



```
<?php
call_user_func_array(array($this->stmt, "bind_param"),refValues($this->valores));
?>
```
<br><br>in this way my db functions keep working in php 5.2/5.3 servers.<br><br>I hope this help someone.  

#

When dealing with a dynamic number of field values while preparing a statement I find this class useful.<br><br>

```
<?php
class BindParam{
    private $values = array(), $types = '';
    
    public function add( $type, &amp;$value ){
        $this->values[] = $value;
        $this->types .= $type;
    }
    
    public function get(){
        return array_merge(array($this->types), $this->values);
    }
}
?>
```


Usage is pretty simple. Create an instance and use the add method to populate. When you're ready to execute simply use the get method.



```
<?php
$bindParam = new BindParam();
$qArray = array();

$use_part_1 = 1;
$use_part_2 = 1;
$use_part_3 = 1;

$query = 'SELECT * FROM users WHERE ';
if($use_part_1){
    $qArray[] = 'hair_color = ?';
    $bindParam->add('s', 'red');
}
if($use_part_2){
    $qArray[] = 'age = ?';
    $bindParam->add('i', 25);
}
if($use_part_3){
    $qArray[] = 'balance = ?';
    $bindParam->add('d', 50.00);
}

$query .= implode(' OR ', $qArray);

//call_user_func_array( array($stm, 'bind_param'), $bindParam->get());

echo $query . '<br/>';
var_dump($bindParam->get());
?>
```
<br><br>This gets you the result that looks something like this:<br><br>SELECT * FROM users WHERE hair_color = ? OR age = ? OR balance = ?<br>array(4) { [0]=&gt; string(3) "sid" [1]=&gt; string(3) "red" [2]=&gt; int(25) [3]=&gt; float(50) }<br><br>[Editor&apos;s note: changed BindParam::add() to accept $value by reference and thereby prevent a warning in newer versions of PHP.]  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.bind-param.php)

**[To root](/README.md)**