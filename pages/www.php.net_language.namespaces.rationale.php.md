# Namespaces overview



Thought this might help other newbies like me...<br><br>Name collisions means: <br>you create a function named db_connect, and somebody elses code that you use in your file (i.e. an include) has the same function with the same name.<br><br>To get around that problem, you rename your function SteveWa_db_connect  which makes your code longer and harder to read.<br><br>Now you can use namespaces to keep your function name separate from anyone else&apos;s function name, and you won&apos;t have to make extra_long_named functions to get around the name collision problem.<br><br>So a namespace is like a pointer to a file path where you can find the source of the function you are working with  

#

Just a note: namespace (even nested or sub-namespace) cannot be just a number, it must start with a letter.<br>For example, lets say you want to use namespace for versioning of your packages or versioning of your API:<br><br>namespace Mynamespace\1;  // Illegal<br>Instead use this:<br>namespace Mynamespace\v1; // OK  

#

To people coming here by searching about namespaces, know that a consortium has studied about best practices in PHP, in order to allow developers to have common coding standards.<br> <br>These best practices are called "PHP Standard Recommendations" , also known as PSR.<br> <br>They are visible on this link : http://www.php-fig.org/psr<br> <br>Actually there are 5 coding standards categories : <br>PSR-0 : Autoloading Standard , which goal is to make the use of Namespaces easier, in order to convert a namespace into a file path.<br>PSR-1 : Basic Coding Standard , basically, standards :) <br>PSR-2 : Coding Style Guide, where to put braces, how to write a class, etc.<br>PSR-3 : Logger Interface , how to write a standard logger<br>PSR-4 : Improved Autoloading , to resolve more Namespaces into paths.<br> <br>The ones I want to point are PSR-0 and PSR-4 : they use namespaces to resolve a FQCN (Fully qualified class name = full namespace + class name) into a file path. <br>Basic example, you have this directory structure :<br>./src/Pierstoval/Tools/MyTool.php<br><br>The namespacing PSR-0 or PSR-4 standard tells that you can transform this path into a FQCN.<br>Read the principles of autoload if you need to know what it means, because it&apos;s almost mandatory ;) .<br><br>Structure :<br>{path}/autoloader.php<br>{path}/index.php<br>{path}/src/Pierstoval/Tools/MyTool.php<br><br>Files :<br><br>

```
<?php
    // {path}/index.php
    include &apos;autoloader.php&apos;;
    $tool = new Pierstoval/Tools/MyTool();
?>
```




```
<?php
    // {path}/src/Pierstoval/Tools/MyTool.php
    namespace Pierstoval\Tools;
    class MyTool {}
?>
```




```
<?php
    // {path}/autoloader.php
    function loadClass($className) {
        $fileName = &apos;&apos;;
        $namespace = &apos;&apos;;

        // Sets the include path as the "src" directory
        $includePath = dirname(__FILE__).DIRECTORY_SEPARATOR.&apos;src&apos;;

        if (false !== ($lastNsPos = strripos($className, &apos;\\&apos;))) {
            $namespace = substr($className, 0, $lastNsPos);
            $className = substr($className, $lastNsPos + 1);
            $fileName = str_replace(&apos;\\&apos;, DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
        }
        $fileName .= str_replace(&apos;_&apos;, DIRECTORY_SEPARATOR, $className) . &apos;.php&apos;;
        $fullFileName = $includePath . DIRECTORY_SEPARATOR . $fileName;
        
        if (file_exists($fullFileName)) {
            require $fullFileName;
        } else {
            echo &apos;Class "&apos;.$className.&apos;" does not exist.&apos;;
        }
    }
    spl_autoload_register(&apos;loadClass&apos;); // Registers the autoloader
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
{path}/src/Pierstoval/Tools/MyTool.php<br><br>This might be the best practices ever in PHP framework developments, such as Symfony or others.  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.rationale.php)

**[To root](/README.md)**