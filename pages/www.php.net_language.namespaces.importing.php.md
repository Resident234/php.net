# Using namespaces: Aliasing/Importing



The keyword "use" has been recycled for three distinct applications: <br>1- to import/alias classes, traits, constants, etc. in namespaces, <br>2- to insert traits in classes, <br>3- to inherit variables in closures. <br>This page is only about the first application: importing/aliasing. Traits can be inserted in classes, but this is different from importing a trait in a namespace, which cannot be done in a block scope, as pointed out in example 5. This can be confusing, especially since all searches for the keyword "use" are directed to the documentation here on importing/aliasing.  

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

Note that you can not alias global namespace:<br><br>use \ as test;<br><br>echo test\strlen(&apos;&apos;);<br><br>won&apos;t work.  

#

Here is a handy way of importing classes, functions and conts using a single use keyword:<br><br>

```
<?php
use Mizo\Web\ {
   Php\WebSite,
   Php\KeyWord,
   Php\UnicodePrint,
   JS\JavaScript, 
   function JS\printTotal, 
   function JS\printList, 
   const JS\BUAIKUM, 
   const JS\MAUTAM
};
?>
```
  

#

I couldn&apos;t find answer to this question so I tested myself. <br>I think it&apos;s worth noting:<br><br>

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

Note the code `use ns1\c1` may refer to importing class `c1` from namespace `ns1` as well as importing whole namespace `ns1\c1` or even import both of them in one line. Example:<br><br>

```
<?php<br>namespace ns1;<br><br>class c1{}<br><br>namespace ns1\c1;<br><br>class c11{}<br><br>namespace main;<br><br>use ns1\c1;<br><br>$c1 = new c1();<br>$c11 = new c1\c11();<br><br>var_dump($c1); // object(ns1\c1)#1 (0) { }<br>var_dump($c11); // object(ns1\c1\c11)#2 (0) { }  

#

If you are testing your code at the CLI, note that namespace aliases do not work!<br><br>(Before I go on, all the backslashes in this example are changed to percent signs because I cannot get sensible results to display in the posting preview otherwise. Please mentally translate all percent signs henceforth as backslashes.)<br><br>Suppose you have a class you want to test in myclass.php:<br><br>

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
<br>it will work fine.<br><br>I hope this reduces the number of prematurely bald people.  

#

Something that is not immediately obvious, particular with PHP 5.3, is that namespace resolutions within an import are not resolved recursively.  i.e.: if you alias an import and then use that alias in another import then this latter import will not be fully resolved with the former import.<br><br>For example:<br>use \Controllers as C;<br>use C\First;<br>use C\Last;<br><br>Both the First and Last namespaces are NOT resolved as \Controllers\First or \Controllers\Last as one might intend.  

#

You are allowed to "use" the same resource multiple times as long as it is imported under a different alias at each invocation.<br><br>For example:<br><br>

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
<br><br>In the above example, "Keller", "Stellar", and "Zellar" are all references to "\Lend\l1\Keller", as are "Lend\l1\Keller", "l1\Keller", and "l3\Keller".  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.importing.php)

**[To root](/README.md)**