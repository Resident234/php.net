# break





A break statement that is in the outer part of a program (e.g. not in a control loop) will end the script. This caught me out when I mistakenly had a break in an if statement

i.e.



```
<?php 
echo &quot;hello&quot;;
if (true) break;
echo &quot; world&quot;; 
?>
```


will only show &quot;hello&quot;

  

#



vlad at vlad dot neosurge dot net wrote on 04-Jan-2003 04:21

&gt; Just an insignificant side not: Like in C/C++, it&apos;s not 
&gt; necessary to break out of the default part of a switch 
&gt; statement in PHP.

It&apos;s not necessary to break out of any case of a switch&#xA0; statement in PHP, but if you want only one case to be executed, you have do break out of it (even out of the default case).

Consider this:



```
<?php
$a = &apos;Apple&apos;;
switch ($a) {
&#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;$a is not an orange&lt;br&gt;&apos;;
&#xA0; &#xA0; case &apos;Orange&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;$a is an orange&apos;;
}
?>
```


This prints (in PHP 5.0.4 on MS-Windows):
$a is not an orange
$a is an orange

Note that the PHP documentation does not state the default part must be the last case statement.

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.break.php)

**[To root](/README.md)**