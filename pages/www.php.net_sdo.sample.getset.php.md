# Setting and Getting Property Values



You can also use curly brackets to evaluate property names (e.g., to access a property name containing special [non-standard] characters without first needing to assign the name to a variable).<br><br>

```
<?php

$object = new StdClass();

// standard access using variable
$nonStandardPropertyName = '/a/path?for=example';
$object->$nonStandardPropertyName = 'access using a variable';

// alternative access using curly braces
$object->{'/a/path?for=example'} = 'set using curly braces';?>
```
  

---

See also: $this pseudo-variable, http://php.net/manual/en/language.oop5.basic.php<br><br>In addition, when inside a class method function, indirect access to object properties also works:<br><br>

```
<?php
    $name = 'departments';
    $departments = $this->$name;
    $this->$name = $different;
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/sdo.sample.getset.php)

**[To root](/README.md)**