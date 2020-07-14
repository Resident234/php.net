# Using namespaces: Basics



Syntax for extending classes in namespaces is still the same.<br><br>Lets call this Object.php:<br><br>

```
<?php

namespace com\rsumilang\common;

class Object{
   // ... code ...
}

?>
```


And now lets create a class called String that extends object in String.php:



```
<?php

class String extends com\rsumilang\common\Object{
   // ... code ...
}

?>
```


Now if you class String was defined in the same namespace as Object then you don't have to specify a full namespace path:



```
<?php

namespace com\rsumilang\common;

class String extends Object
{
   // ... code ...
}

?>
```


Lastly, you can also alias a namespace name to use a shorter name for the class you are extending incase your class is in seperate namespace:



```
<?php

namespace com\rsumilang\util;
use com\rsumlang\common as Common;

class String extends Common\Object
{
   // ... code ...
}

?>
```
<br><br>- Richard Sumilang  

#



```
<?php

namespace Foo;

try {
    // Something awful here
    // That will throw a new exception from SPL
} 
catch (Exception as $ex) {
    // We will never get here
    // This is because we are catchin Foo\Exception
}
?>
```


Instead use fully qualified name for the exception to catch it



```
<?php 

namespace Foo;

try {
    // something awful here
    // That will throw a new exception from SPL
} 
catch (\Exception as $ex) {
    // Now we can get here at last
}
?>
```
  

#

Well variables inside namespaces do not override others since variables are never affected by namespace but always global:<br>"Although any valid PHP code can be contained within a namespace, only four types of code are affected by namespaces: classes, interfaces, functions and constants. "<br><br>Source: "Defining Namespaces"<br>http://www.php.net/manual/en/language.namespaces.definition.php  

#

It seems the file system analogy only goes so far. One thing that&apos;s missing that would be very useful is relative navigation up the namespace chain, e.g.<br><br>

```
<?php
namespace MyProject {
   class Person {}
}

namespace MyProject\People {
    class Adult extends ..\Person {}
}
?>
```
<br><br>That would be really nice, especially if you had really deep namespaces. It would save you having to type out the full namespace just to reference a resource one level up.  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.basics.php)

**[To root](/README.md)**