# Defining namespaces





If your code looks like this:



```
<?php
&#xA0; &#xA0; namespace NS;
?>
```


...and you still get &quot;Namespace declaration statement has to be the very first statement in the script&quot; Fatal error, then you probably use UTF-8 encoding (which is good) with Byte Order Mark, aka BOM (which is bad). Try to convert your files to &quot;UTF-8 without BOM&quot;, and it should be ok.

  

#



Regarding constants defined with define() inside namespaces...

define() will define constants exactly as specified.&#xA0; So, if you want to define a constant in a namespace, you will need to specify the namespace in your call to define(), even if you&apos;re calling define() from within a namespace.&#xA0; The following examples will make it clear.

The following code will define the constant &quot;MESSAGE&quot; in the global namespace (i.e. &quot;\MESSAGE&quot;).



```
<?php
namespace test;
define(&apos;MESSAGE&apos;, &apos;Hello world!&apos;);
?>
```


The following code will define two constants in the &quot;test&quot; namespace.



```
<?php
namespace test;
define(&apos;test\HELLO&apos;, &apos;Hello world!&apos;);
define(__NAMESPACE__ . &apos;\GOODBYE&apos;, &apos;Goodbye cruel world!&apos;);
?>
```



  

#



Expanding on @danbettles note, it is better to always be explicit about which constant to use.



```
<?php
&#xA0; &#xA0; namespace NS;

&#xA0; &#xA0; define(__NAMESPACE__ .&apos;\foo&apos;,&apos;111&apos;);
&#xA0; &#xA0; define(&apos;foo&apos;,&apos;222&apos;);

&#xA0; &#xA0; echo foo;&#xA0; // 111.
&#xA0; &#xA0; echo \foo;&#xA0; // 222.
&#xA0; &#xA0; echo \NS\foo&#xA0; // 111.
&#xA0; &#xA0; echo NS\foo&#xA0; // fatal error. assumes \NS\NS\foo.
?>
```



  

#



&quot;A file containing a namespace must declare the namespace at the top of the file before any other code&quot;



It might be obvious, but this means that you *can* include comments and white spaces before the namespace keyword.





```
<?php

// Lots 

// of

// interesting

// comments and white space



namespace Foo;

class Bar {

}

?>
```



  

#



You should not try to create namespaces that use PHP keywords. These will cause parse errors. 



Examples:





```
<?php

namespace Project/Classes/Function; // Causes parse errors

namespace Project/Abstract/Factory; // Causes parse errors

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definition.php)

**[To root](/README.md)**