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
    $t = &apos;&apos;;
    $args = func_get_args();
    foreach ($args as &amp;$a) {
        $t .= gettype($a) . &apos;|&apos;;
        $a = mysql_real_escape_string($a);
    }
    if ($t != &apos;&apos;) {
        $t = substr($t, 0, - 1);
    }
    $sql = &apos;&apos;;
    switch ($t) {
        case &apos;integer&apos;:
            // search by ID
            $sql = "id = {$args[0]}";
            break;
        case &apos;string&apos;:
            // search by name
            $sql = "name LIKE &apos;%{$args[0]}%&apos;";
            break;
        case &apos;string|integer&apos;:
            // search by name AND status
            $sql = "name LIKE &apos;%{$args[0]}%&apos; AND status = {$args[1]}";
            break;
        case &apos;string|integer|integer&apos;:
            // search by name with limit
            $sql = "name LIKE &apos;%{$args[0]}%&apos; LIMIT {$args[1]},{$args[2]}";
            break;
        default:
            // :P
            $sql = &apos;1 = 2&apos;;
    }
    return mysql_query(&apos;SELECT * FROM table WHERE &apos; . $sql);
}
$res = select(29); // by ID
$res = select(&apos;Anderson&apos;); // by name
$res = select(&apos;Anderson&apos;, 1); // by name and status
$res = select(&apos;Anderson&apos;, 0, 5); // by name with limit
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.func-get-args.php)

**[To root](/README.md)**