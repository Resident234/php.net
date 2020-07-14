# Unsetting References



Simple look how PHP Reference works<br>

```
<?php
/* Imagine this is memory map
 ______________________________
|pointer | value | variable              |
 -----------------------------------
|   1     |  NULL  |         ---           |
|   2     |  NULL  |         ---           |
|   3     |  NULL  |         ---           |
|   4     |  NULL  |         ---           |
|   5     |  NULL  |         ---           |
------------------------------------
Create some variables   */
$a=10;
$b=20;
$c=array ('one'=>array (1, 2, 3));
/* Look at memory
 _______________________________
|pointer | value |       variable's       |
 -----------------------------------
|   1     |  10     |       $a               |
|   2     |  20     |       $b               |
|   3     |  1       |      $c['one'][0]   |
|   4     |  2       |      $c['one'][1]   |
|   5     |  3       |      $c['one'][2]   |
------------------------------------
do  */
$a=&amp;$c['one'][2];
/* Look at memory
 _______________________________
|pointer | value |       variable's       |
 -----------------------------------
|   1     |  NULL  |       ---              |  //value of  $a is destroyed and pointer is free
|   2     |  20     |       $b               |
|   3     |  1       |      $c['one'][0]   |
|   4     |  2       |      $c['one'][1]   |
|   5     |  3       |  $c['one'][2]  ,$a | // $a is now here
------------------------------------
do  */
$b=&amp;$a;  // or  $b=&amp;$c['one'][2]; result is same as both "$c['one'][2]" and "$a" is at same pointer.
/* Look at memory
 _________________________________
|pointer | value |       variable's           |
 --------------------------------------
|   1     |  NULL  |       ---                  |  
|   2     |  NULL  |       ---                  |  //value of  $b is destroyed and pointer is free
|   3     |  1       |      $c['one'][0]       |
|   4     |  2       |      $c['one'][1]       |
|   5     |  3       |$c['one'][2]  ,$a , $b |  // $b is now here
---------------------------------------
next do */
unset($c['one'][2]);
/* Look at memory
 _________________________________
|pointer | value |       variable's           |
 --------------------------------------
|   1     |  NULL  |       ---                  |  
|   2     |  NULL  |       ---                  |  
|   3     |  1       |      $c['one'][0]       |
|   4     |  2       |      $c['one'][1]       |
|   5     |  3       |      $a , $b              | // $c['one'][2]  is  destroyed not in memory, not in array
---------------------------------------
next do   */
$c['one'][2]=500;    //now it is in array
/* Look at memory
 _________________________________
|pointer | value |       variable's           |
 --------------------------------------
|   1     |  500    |      $c['one'][2]       |  //created it lands on any(next) free pointer in memory
|   2     |  NULL  |       ---                  |  
|   3     |  1       |      $c['one'][0]       |
|   4     |  2       |      $c['one'][1]       |
|   5     |  3       |      $a , $b              | //this pointer is in use
---------------------------------------
lets tray to return $c['one'][2] at old pointer an remove reference $a,$b.  */
$c['one'][2]=&amp;$a;
unset($a);
unset($b);   
/* look at memory
 _________________________________
|pointer | value |       variable's           |
 --------------------------------------
|   1     |  NULL  |       ---                  |  
|   2     |  NULL  |       ---                  |  
|   3     |  1       |      $c['one'][0]       |
|   4     |  2       |      $c['one'][1]       |
|   5     |  3       |      $c['one'][2]       | //$c['one'][2] is returned, $a,$b is destroyed
--------------------------------------- ?>
```
<br>I hope this helps.  

#



```
<?php
//if you do:

$a = "hihaha";
$b = &amp;$a;
$c = "eita";
$b = $c;
echo $a; // shows "eita"

$a = "hihaha";
$b = &amp;$a;
$c = "eita";
$b = &amp;$c;
echo $a; // shows "hihaha"

$a = "hihaha";
$b = &amp;$a;
$b = null;
echo $a; // shows nothing (both are set to null)

$a = "hihaha";
$b = &amp;$a;
unset($b);
echo $a; // shows "hihaha"

$a = "hihaha";
$b = &amp;$a;
$c = "eita";
$a = $c;
echo $b; // shows "eita"

$a = "hihaha";
$b = &amp;$a;
$c = "eita";
$a = &amp;$c;
echo $b; // shows "hihaha"

$a = "hihaha";
$b = &amp;$a;
$a = null;
echo $b; // shows nothing (both are set to null)

$a = "hihaha";
$b = &amp;$a;
unset($a);
echo $b; // shows "hihaha"
?>
```
<br><br>I tested each case individually on PHP 4.3.10.  

#

[Official documentation page](https://www.php.net/manual/en/language.references.unset.php)

**[To root](/README.md)**