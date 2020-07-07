# Namespaces overview





Thought this might help other newbies like me...

Name collisions means: 
you create a function named db_connect, and somebody elses code that you use in your file (i.e. an include) has the same function with the same name.

To get around that problem, you rename your function SteveWa_db_connect&#xA0; which makes your code longer and harder to read.

Now you can use namespaces to keep your function name separate from anyone else&apos;s function name, and you won&apos;t have to make extra_long_named functions to get around the name collision problem.

So a namespace is like a pointer to a file path where you can find the source of the function you are working with

  

#



Just a note: namespace (even nested or sub-namespace) cannot be just a number, it must start with a letter.
For example, lets say you want to use namespace for versioning of your packages or versioning of your API:

namespace Mynamespace\1;&#xA0; // Illegal
Instead use this:
namespace Mynamespace\v1; // OK

  

#



To people coming here by searching about namespaces, know that a consortium has studied about best practices in PHP, in order to allow developers to have common coding standards.
 
These best practices are called &quot;PHP Standard Recommendations&quot; , also known as PSR.
 
They are visible on this link : http://www.php-fig.org/psr
 
Actually there are 5 coding standards categories : 
PSR-0 : Autoloading Standard , which goal is to make the use of Namespaces easier, in order to convert a namespace into a file path.
PSR-1 : Basic Coding Standard , basically, standards :) 
PSR-2 : Coding Style Guide, where to put braces, how to write a class, etc.
PSR-3 : Logger Interface , how to write a standard logger
PSR-4 : Improved Autoloading , to resolve more Namespaces into paths.
 
The ones I want to point are PSR-0 and PSR-4 : they use namespaces to resolve a FQCN (Fully qualified class name = full namespace + class name) into a file path. 
Basic example, you have this directory structure :
./src/Pierstoval/Tools/MyTool.php

The namespacing PSR-0 or PSR-4 standard tells that you can transform this path into a FQCN.
Read the principles of autoload if you need to know what it means, because it&apos;s almost mandatory ;) .

Structure :
{path}/autoloader.php
{path}/index.php
{path}/src/Pierstoval/Tools/MyTool.php

Files :



```
<?php
&#xA0; &#xA0; // {path}/index.php
&#xA0; &#xA0; include &apos;autoloader.php&apos;;
&#xA0; &#xA0; $tool = new Pierstoval/Tools/MyTool();
?>
```




```
<?php
&#xA0; &#xA0; // {path}/src/Pierstoval/Tools/MyTool.php
&#xA0; &#xA0; namespace Pierstoval\Tools;
&#xA0; &#xA0; class MyTool {}
?>
```




```
<?php
&#xA0; &#xA0; // {path}/autoloader.php
&#xA0; &#xA0; function loadClass($className) {
&#xA0; &#xA0; &#xA0; &#xA0; $fileName = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $namespace = &apos;&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; // Sets the include path as the &quot;src&quot; directory
&#xA0; &#xA0; &#xA0; &#xA0; $includePath = dirname(__FILE__).DIRECTORY_SEPARATOR.&apos;src&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; if (false !== ($lastNsPos = strripos($className, &apos;\\&apos;))) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $namespace = substr($className, 0, $lastNsPos);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $className = substr($className, $lastNsPos + 1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fileName = str_replace(&apos;\\&apos;, DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $fileName .= str_replace(&apos;_&apos;, DIRECTORY_SEPARATOR, $className) . &apos;.php&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $fullFileName = $includePath . DIRECTORY_SEPARATOR . $fileName;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if (file_exists($fullFileName)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; require $fullFileName;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Class &quot;&apos;.$className.&apos;&quot; does not exist.&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; spl_autoload_register(&apos;loadClass&apos;); // Registers the autoloader
?>
```

 
A standardized autoloader will get the class you want to instanciate (MyTool) and get the FQCN, transform it into a file path, and check if the file exists. If it does, it will 

```
<?php include(); ?>
```
 it, and if you wrote your class correctly, the class will be available within its correct namespace.
Then, if you have the following code :


```
<?php $tool = new Pierstoval/Tools/MyTool(); ?>
```

The autoloader will transform the FQCN into this path :
{path}/src/Pierstoval/Tools/MyTool.php

This might be the best practices ever in PHP framework developments, such as Symfony or others.

  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.rationale.php)

**[To root](/README.md)**