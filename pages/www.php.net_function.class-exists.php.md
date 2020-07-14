# class_exists



If you are using aliasing to import namespaced classes, take care that class_exists will not work using the short, aliased class name - apparently whenever a class name is used as string, only the full-namespace version can be used<br><br>use a\namespaced\classname as coolclass;<br><br>class_exists( &apos;coolclass&apos; ) =&gt; false  

#

Beware: class_exists is case-INsensitive, as is class instantiation.<br><br>php &gt; var_dump(class_exists("DomNode"));<br>bool(true)<br>php &gt; var_dump(class_exists("DOMNode"));<br>bool(true)<br>php &gt; var_dump(class_exists("DOMNodE"));<br>bool(true)<br>php &gt; $x = new DOMNOdE();<br>php &gt; var_dump(get_class($x));<br>string(7) "DOMNode"<br><br>(tested with PHP 5.5.10 on Linux)<br><br>This can cause some headaches in correlating class names to file names, especially on a case-sensitive file system.  

#

If you recursively load several classes inside an autoload function (or mix manual loading and autoloading), be aware that class_exists() (as well as get_declared_classes()) does not know about classes previously loaded during the *current* autoload invocation.<br><br>Apparently, the internal list of declared classes is only updated after the autoload function is completed.  

#

Hi guys!<br>Be careful  and don&apos;t forget about second boolean argument $autoload (TRUE by default) when check exists class after spl_autoload_register. Propose short example<br>file second.php<br>

```
<?php
class Second {}
?>
```

file index.php


```
<?php
class First
{
    function first($class, $bool) {
        spl_autoload_register( function($class) {
            require strtolower($class) . &apos;.php&apos;;
        });
        echo class_exists($class, $bool)?&apos;Exist!!!!&apos;:&apos;Not exist!&apos;;
    }
}

new First($class = &apos;Second&apos;, $bool = true); //Exist!!!!
new First($class = &apos;Second&apos;, $bool = false); //Not exist!
?>
```
<br>Because __autoload executing much earlier than boolean returned, imho..  

#

[Official documentation page](https://www.php.net/manual/en/function.class-exists.php)

**[To root](/README.md)**