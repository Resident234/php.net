# unserialize



Just some reminder which may save somebody some time regarding the `$options` array: <br><br>Say you want to be on the safe side and not allow any objects to be unserialized... My first thought was doing the following:<br><br>

```
<?php
$lol = unserialize($string, false);
// This will generate:
// Warning: unserialize() expects parameter 2 to be array, boolean given
?>
```


The correct way of doing this is the following:


```
<?php
$lol = unserialize($string, ['allowed_classes' => false]);
?>
```
<br><br>Hope it helps somebody!  

#

Just a note - if the serialized string contains a reference to a class that cannot be instantiated (e.g. being abstract) PHP will immediately die with a fatal error. If the unserialize() statement is preceded with a &apos;@&apos; to avoid cluttering the logs with warns or notices there will be absolutely no clue as to why the script stopped working. Cost me a couple of hours...  

#

In the Classes and Objects docs, there is this: In order to be able to unserialize() an object, the class of that object needs to be defined.<br><br>Prior to PHP 5.3, this was not an issue.  But after PHP 5.3 an object made by SimpleXML_Load_String() cannot be serialized.  An attempt to do so will result in a run-time failure, throwing an exception.  If you store such an object in $_SESSION, you will get a post-execution error that says this:<br><br>Fatal error: Uncaught exception &apos;Exception&apos; with message &apos;Serialization of &apos;SimpleXMLElement&apos; is not allowed&apos; in [no active file]:0 Stack trace: #0 {main} thrown in [no active file] on line 0<br><br>The entire contents of the session will be lost.  Hope this saves someone some time!<br><br>

```
<?php // RAY_temp_ser.php
error_reporting(E_ALL);
session_start();
var_dump($_SESSION);
$_SESSION['hello'] = 'World';
var_dump($_SESSION);

// AN XML STRING FOR TEST DATA
$xml = '&lt;?xml version="1.0"?>
```
<br>&lt;families&gt;<br>  &lt;parent&gt;<br>    &lt;child index="1" value="Category 1"&gt;Child One&lt;/child&gt;<br>  &lt;/parent&gt;<br>&lt;/families&gt;&apos;;<br><br>// MAKE AN OBJECT (GIVES SimpleXMLElement)<br>$obj = SimpleXML_Load_String($xml);<br><br>// STORE THE OBJECT IN THE SESSION<br>$_SESSION[&apos;obj&apos;] = $obj;  

#

[Official documentation page](https://www.php.net/manual/en/function.unserialize.php)

**[To root](/README.md)**