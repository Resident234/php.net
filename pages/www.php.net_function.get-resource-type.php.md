# get_resource_type



Try this to know behavior:<br><br>

```
<?php
function resource_test($resource, $name) {
    echo 
        '[' . $name. ']',
        PHP_EOL,
        '(bool)$resource => ',
        $resource ? 'TRUE' : 'FALSE',
        PHP_EOL,
        'get_resource_type($resource) => ',
        get_resource_type($resource) ?: 'FALSE',
        PHP_EOL,
        'is_resoruce($resource) => ',
        is_resource($resource) ? 'TRUE' : 'FALSE',
        PHP_EOL,
        PHP_EOL
    ;
}
 
$resource = tmpfile();
resource_test($resource, 'Check Valid Resource');
 
fclose($resource);
resource_test($resource, 'Check Released Resource');
 
$resource = null;
resource_test($resource, 'Check NULL');
?>
```
<br><br>It will be shown as...<br><br>[Check Valid Resource]<br>(bool)$resource =&gt; TRUE<br>get_resource_type($resource) =&gt; stream<br>is_resoruce($resource) =&gt; TRUE<br><br>[Check Released Resource]<br>(bool)$resource =&gt; TRUE<br>get_resource_type($resource) =&gt; Unknown<br>is_resoruce($resource) =&gt; FALSE<br><br>[Check NULL]<br>(bool)$resource =&gt; FALSE<br>get_resource_type($resource) =&gt; FALSE<br>Warning:  get_resource_type() expects parameter 1 to be resource, null given in ... on line 10<br>is_resoruce($resource) =&gt; FALSE  

#

[Official documentation page](https://www.php.net/manual/en/function.get-resource-type.php)

**[To root](/README.md)**