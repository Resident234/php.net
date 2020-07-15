# func_get_args



Simple function to calculate average value using dynamic arguments:<br>

```
<?php
function average(){
    return array_sum(func_get_args())/func_num_args();
}
print average(10, 15, 20, 25); // 17.5
?>
```
  

#

How to create a polymorphic/"overloaded" function<br><br>

```
<?php
function select()
{
    $t = '';
    $args = func_get_args();
    foreach ($args as &amp;$a) {
        $t .= gettype($a) . '|';
        $a = mysql_real_escape_string($a);
    }
    if ($t != '') {
        $t = substr($t, 0, - 1);
    }
    $sql = '';
    switch ($t) {
        case 'integer':
            // search by ID
            $sql = "id = {$args[0]}";
            break;
        case 'string':
            // search by name
            $sql = "name LIKE '%{$args[0]}%'";
            break;
        case 'string|integer':
            // search by name AND status
            $sql = "name LIKE '%{$args[0]}%' AND status = {$args[1]}";
            break;
        case 'string|integer|integer':
            // search by name with limit
            $sql = "name LIKE '%{$args[0]}%' LIMIT {$args[1]},{$args[2]}";
            break;
        default:
            // :P
            $sql = '1 = 2';
    }
    return mysql_query('SELECT * FROM table WHERE ' . $sql);
}
$res = select(29); // by ID
$res = select('Anderson'); // by name
$res = select('Anderson', 1); // by name and status
$res = select('Anderson', 0, 5); // by name with limit
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.func-get-args.php)

**[To root](/README.md)**