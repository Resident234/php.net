# class_implements



You can also check if a class implements an interface using instanceof.<br><br>E.g.<br>

```
<?php
if($myObj instanceof MyInterface) {
    echo "It is! It is!";
}
?>
```
  

#

Hint:<br>

```
<?php
in_array("your-interface", class_implements($object_or_class_name));
?>
```

would check if 'your-interface' is ONE of the implemented interfaces.
Note that you can use something similar to be sure the class only implements that, (whyever you would want that?)


```
<?php
array("your-interface") == class_implements($object_or_class_name);
?>
```
<br><br>I use the first technique to check if a module has the correct interface implemented, or else it throws an exception.  

#

[Official documentation page](https://www.php.net/manual/en/function.class-implements.php)

**[To root](/README.md)**