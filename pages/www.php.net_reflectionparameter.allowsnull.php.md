# ReflectionParameter::allowsNull



The allowsNull method look if arguments have a type. <br>If a type is defined, null is allowed only if default value is null.<br><br> 

```
<?php 
function myfunction ( $param ) {
    
}

echo (new ReflectionFunction("myfunction"))->getParameters()[0]->allowsNull() ? "true":"false";

?>
```


Result : true



```
<?php 
function myfunction ( stdClass $param ) {
    
}

echo (new ReflectionFunction("myfunction"))->getParameters()[0]->allowsNull() ? "true":"false";

?>
```


Result : false



```
<?php
function myfunction ( stdClass $param = null ) {
    
}

echo (new ReflectionFunction("myfunction"))->getParameters()[0]->allowsNull() ? "true":"false";
?>
```
<br><br>Result : true  

#

[Official documentation page](https://www.php.net/manual/en/reflectionparameter.allowsnull.php)

**[To root](/README.md)**