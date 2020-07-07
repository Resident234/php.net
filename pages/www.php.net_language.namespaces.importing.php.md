# Using namespaces: Aliasing/Importing





The keyword &quot;use&quot; has been recycled for three distinct applications: 
1- to import/alias classes, traits, constants, etc. in namespaces, 
2- to insert traits in classes, 
3- to inherit variables in closures. 
This page is only about the first application: importing/aliasing. Traits can be inserted in classes, but this is different from importing a trait in a namespace, which cannot be done in a block scope, as pointed out in example 5. This can be confusing, especially since all searches for the keyword &quot;use&quot; are directed to the documentation here on importing/aliasing.

  

#



The 

```
<?php use ?>
```
 statement does not load the class file. You have to do this with the 

```
<?php require ?>
```
 statement or by using an autoload function.

  

#



Note that you can not alias global namespace:

use \ as test;

echo test\strlen(&apos;&apos;);

won&apos;t work.

  

#



Here is a handy way of importing classes, functions and conts using a single use keyword:



```
<?php
use Mizo\Web\ {
&#xA0;&#xA0; Php\WebSite,
&#xA0;&#xA0; Php\KeyWord,
&#xA0;&#xA0; Php\UnicodePrint,
&#xA0;&#xA0; JS\JavaScript, 
&#xA0;&#xA0; function JS\printTotal, 
&#xA0;&#xA0; function JS\printList, 
&#xA0;&#xA0; const JS\BUAIKUM, 
&#xA0;&#xA0; const JS\MAUTAM
};
?>
```



  

#



I couldn&apos;t find answer to this question so I tested myself. 
I think it&apos;s worth noting:



```
<?php
use ExistingNamespace\NonExsistingClass;
use ExistingNamespace\NonExsistingClass as whatever;
use NonExistingNamespace\NonExsistingClass;
use NonExistingNamespace\NonExsistingClass as whatever;
?>
```


None of above will actually cause errors unless you actually try to use class you tried to import. 



```
<?php
// And this code will issue standard PHP error for non existing class.
use ExistingNamespace\NonExsistingClass as whatever;
$whatever = new whatever();
?>
```



  

#



Note the code `use ns1\c1` may refer to importing class `c1` from namespace `ns1` as well as importing whole namespace `ns1\c1` or even import both of them in one line. Example:



```
<?php
namespace ns1;

class c1{}

namespace ns1\c1;

class c11{}

namespace main;

use ns1\c1;

$c1 = new c1();
$c11 = new c1\c11();

var_dump($c1); // object(ns1\c1)#1 (0) { }
var_dump($c11); // object(ns1\c1\c11)#2 (0) { }


  

#



If you are testing your code at the CLI, note that namespace aliases do not work!

(Before I go on, all the backslashes in this example are changed to percent signs because I cannot get sensible results to display in the posting preview otherwise. Please mentally translate all percent signs henceforth as backslashes.)

Suppose you have a class you want to test in myclass.php:



```
<?php
namespace my%space;
class myclass {
 // ...
}
?>
```


and you then go into the CLI to test it. You would like to think that this would work, as you type it line by line:

require &apos;myclass.php&apos;;
use my%space%myclass; // should set &apos;myclass&apos; as alias for &apos;my%space%myclass&apos;
$x = new myclass; // FATAL ERROR

I believe that this is because aliases are only resolved at compile time, whereas the CLI simply evaluates statements; so use statements are ineffective in the CLI.

If you put your test code into test.php:


```
<?php
require &apos;myclass.php&apos;;
use my%space%myclass;
$x = new myclass;
//...
?>
```

it will work fine.

I hope this reduces the number of prematurely bald people.

  

#



Something that is not immediately obvious, particular with PHP 5.3, is that namespace resolutions within an import are not resolved recursively.&#xA0; i.e.: if you alias an import and then use that alias in another import then this latter import will not be fully resolved with the former import.

For example:
use \Controllers as C;
use C\First;
use C\Last;

Both the First and Last namespaces are NOT resolved as \Controllers\First or \Controllers\Last as one might intend.

  

#



You are allowed to &quot;use&quot; the same resource multiple times as long as it is imported under a different alias at each invocation.

For example:



```
<?php
use Lend;
use Lend\l1;
use Lend\l1 as l3;
use Lend\l2;
use Lend\l1\Keller;
use Lend\l1\Keller as Stellar;
use Lend\l1\Keller as Zellar;
use Lend\l2\Keller as Dellar;

...

?>
```


In the above example, &quot;Keller&quot;, &quot;Stellar&quot;, and &quot;Zellar&quot; are all references to &quot;\Lend\l1\Keller&quot;, as are &quot;Lend\l1\Keller&quot;, &quot;l1\Keller&quot;, and &quot;l3\Keller&quot;.

  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.importing.php)

**[To root](/README.md)**