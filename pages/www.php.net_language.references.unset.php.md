# Unsetting References





Simple look how PHP Reference works


```
<?php
/* Imagine this is memory map
 ______________________________
|pointer | value | variable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
 -----------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
------------------------------------
Create some variables&#xA0;&#xA0; */
$a=10;
$b=20;
$c=array (&apos;one&apos;=&gt;array (1, 2, 3));
/* Look at memory
 _______________________________
|pointer | value |&#xA0; &#xA0; &#xA0;&#xA0; variable&apos;s&#xA0; &#xA0; &#xA0;&#xA0; |
 -----------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; 10&#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; $a&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; 20&#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; $b&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; 1&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][0]&#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; 2&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][1]&#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; 3&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][2]&#xA0;&#xA0; |
------------------------------------
do&#xA0; */
$a=&amp;$c[&apos;one&apos;][2];
/* Look at memory
 _______________________________
|pointer | value |&#xA0; &#xA0; &#xA0;&#xA0; variable&apos;s&#xA0; &#xA0; &#xA0;&#xA0; |
 -----------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; //value of&#xA0; $a is destroyed and pointer is free
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; 20&#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; $b&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; 1&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][0]&#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; 2&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][1]&#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; 3&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; $c[&apos;one&apos;][2]&#xA0; ,$a | // $a is now here
------------------------------------
do&#xA0; */
$b=&amp;$a;&#xA0; // or&#xA0; $b=&amp;$c[&apos;one&apos;][2]; result is same as both &quot;$c[&apos;one&apos;][2]&quot; and &quot;$a&quot; is at same pointer.
/* Look at memory
 _________________________________
|pointer | value |&#xA0; &#xA0; &#xA0;&#xA0; variable&apos;s&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
 --------------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; 
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; //value of&#xA0; $b is destroyed and pointer is free
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; 1&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][0]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; 2&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][1]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; 3&#xA0; &#xA0; &#xA0;&#xA0; |$c[&apos;one&apos;][2]&#xA0; ,$a , $b |&#xA0; // $b is now here
---------------------------------------
next do */
unset($c[&apos;one&apos;][2]);
/* Look at memory
 _________________________________
|pointer | value |&#xA0; &#xA0; &#xA0;&#xA0; variable&apos;s&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
 --------------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; 
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; 
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; 1&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][0]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; 2&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][1]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; 3&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $a , $b&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | // $c[&apos;one&apos;][2]&#xA0; is&#xA0; destroyed not in memory, not in array
---------------------------------------
next do&#xA0;&#xA0; */
$c[&apos;one&apos;][2]=500;&#xA0; &#xA0; //now it is in array
/* Look at memory
 _________________________________
|pointer | value |&#xA0; &#xA0; &#xA0;&#xA0; variable&apos;s&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
 --------------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; 500&#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][2]&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; //created it lands on any(next) free pointer in memory
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; 
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; 1&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][0]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; 2&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][1]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; 3&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $a , $b&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | //this pointer is in use
---------------------------------------
lets tray to return $c[&apos;one&apos;][2] at old pointer an remove reference $a,$b.&#xA0; */
$c[&apos;one&apos;][2]=&amp;$a;
unset($a);
unset($b);&#xA0;&#xA0; 
/* look at memory
 _________________________________
|pointer | value |&#xA0; &#xA0; &#xA0;&#xA0; variable&apos;s&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
 --------------------------------------
|&#xA0;&#xA0; 1&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; 
|&#xA0;&#xA0; 2&#xA0; &#xA0;&#xA0; |&#xA0; NULL&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; ---&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; 
|&#xA0;&#xA0; 3&#xA0; &#xA0;&#xA0; |&#xA0; 1&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][0]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 4&#xA0; &#xA0;&#xA0; |&#xA0; 2&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][1]&#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0;&#xA0; 5&#xA0; &#xA0;&#xA0; |&#xA0; 3&#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; $c[&apos;one&apos;][2]&#xA0; &#xA0; &#xA0;&#xA0; | //$c[&apos;one&apos;][2] is returned, $a,$b is destroyed
--------------------------------------- ?>
```

I hope this helps.


  

#





```
<?php
//if you do:

$a = &quot;hihaha&quot;;
$b = &amp;$a;
$c = &quot;eita&quot;;
$b = $c;
echo $a; // shows &quot;eita&quot;

$a = &quot;hihaha&quot;;
$b = &amp;$a;
$c = &quot;eita&quot;;
$b = &amp;$c;
echo $a; // shows &quot;hihaha&quot;

$a = &quot;hihaha&quot;;
$b = &amp;$a;
$b = null;
echo $a; // shows nothing (both are set to null)

$a = &quot;hihaha&quot;;
$b = &amp;$a;
unset($b);
echo $a; // shows &quot;hihaha&quot;

$a = &quot;hihaha&quot;;
$b = &amp;$a;
$c = &quot;eita&quot;;
$a = $c;
echo $b; // shows &quot;eita&quot;

$a = &quot;hihaha&quot;;
$b = &amp;$a;
$c = &quot;eita&quot;;
$a = &amp;$c;
echo $b; // shows &quot;hihaha&quot;

$a = &quot;hihaha&quot;;
$b = &amp;$a;
$a = null;
echo $b; // shows nothing (both are set to null)

$a = &quot;hihaha&quot;;
$b = &amp;$a;
unset($a);
echo $b; // shows &quot;hihaha&quot;
?>
```


I tested each case individually on PHP 4.3.10.

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.unset.php)

**[To root](/README.md)**