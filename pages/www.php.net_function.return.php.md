# return



for those of you who think that using return in a script is the same as using exit note that: using return just exits the execution of the current script, exit the whole execution.<br><br>look at that example:<br><br>a.php<br>

```
<?php
include("b.php");
echo "a";
?>
```


b.php


```
<?php
echo "b";
return;
?>
```


(executing a.php:) will echo "ba".

whereas (b.php modified):

a.php


```
<?php
include("b.php");
echo "a";
?>
```


b.php


```
<?php
echo "b";
exit;
?>
```
<br><br>(executing a.php:) will echo "b".  

#

Note that because PHP processes the file before running it, any functions defined in an included file will still be available, even if the file is not executed.<br><br>Example:<br><br>a.php<br>

```
<?php
include &apos;b.php&apos;;

foo();
?>
```


b.php


```
<?php
return;

function foo() {
     echo &apos;foo&apos;;
}
?>
```
<br><br>Executing a.php will output "foo".  

#

[Official documentation page](https://www.php.net/manual/en/function.return.php)

**[To root](/README.md)**