# break



A break statement that is in the outer part of a program (e.g. not in a control loop) will end the script. This caught me out when I mistakenly had a break in an if statement<br><br>i.e.<br><br>

```
<?php 
echo "hello";
if (true) break;
echo " world"; 
?>
```
<br><br>will only show "hello"  

#

vlad at vlad dot neosurge dot net wrote on 04-Jan-2003 04:21<br><br>&gt; Just an insignificant side not: Like in C/C++, it&apos;s not <br>&gt; necessary to break out of the default part of a switch <br>&gt; statement in PHP.<br><br>It&apos;s not necessary to break out of any case of a switch  statement in PHP, but if you want only one case to be executed, you have do break out of it (even out of the default case).<br><br>Consider this:<br><br>

```
<?php
$a = &apos;Apple&apos;;
switch ($a) {
    default:
        echo &apos;$a is not an orange&lt;br&gt;&apos;;
    case &apos;Orange&apos;:
        echo &apos;$a is an orange&apos;;
}
?>
```
<br><br>This prints (in PHP 5.0.4 on MS-Windows):<br>$a is not an orange<br>$a is an orange<br><br>Note that the PHP documentation does not state the default part must be the last case statement.  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.break.php)

**[To root](/README.md)**