# Setting and Getting Property Values



You can also use curly brackets to evaluate property names (e.g., to access a property name containing special [non-standard] characters without first needing to assign the name to a variable).<br><br>

```
<?php<br><br>$object = new StdClass();<br><br>// standard access using variable<br>$nonStandardPropertyName = &apos;/a/path?for=example&apos;;<br>$object-&gt;$nonStandardPropertyName = &apos;access using a variable&apos;;<br><br>// alternative access using curly braces<br>$object-&gt;{&apos;/a/path?for=example&apos;} = &apos;set using curly braces&apos;;  

#

See also: $this pseudo-variable, http://php.net/manual/en/language.oop5.basic.php<br><br>In addition, when inside a class method function, indirect access to object properties also works:<br><br>

```
<?php
    $name = &apos;departments&apos;;
    $departments = $this-&gt;$name;
    $this-&gt;$name = $different;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/sdo.sample.getset.php)

**[To root](/README.md)**