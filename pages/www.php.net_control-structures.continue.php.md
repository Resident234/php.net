# continue



The remark "in PHP the switch statement is considered a looping structure for the purposes of continue" near the top of this page threw me off, so I experimented a little using the following code to figure out what the exact semantics of continue inside a switch is:<br><br>

```
<?php

    for( $i = 0; $i < 3; ++ $i )
    {
        echo ' [', $i, '] ';
        switch( $i )
        {
            case 0: echo 'zero'; break;
            case 1: echo 'one' ; XXXX;
            case 2: echo 'two' ; break;
        }
        echo ' <' , $i, '> ';
    }

?>
```
<br><br>For XXXX I filled in<br><br>- continue 1<br>- continue 2<br>- break 1<br>- break 2<br><br>and observed the different results.  This made me come up with the following one-liner that describes the difference between break and continue:<br><br>continue resumes execution just before the closing curly bracket ( } ), and break resumes execution just after the closing curly bracket.<br><br>Corollary: since a switch is not (really) a looping structure, resuming execution just before a switch&apos;s closing curly bracket has the same effect as using a break statement.  In the case of (for, while, do-while) loops, resuming execution just prior their closing curly brackets means that a new iteration is started --which is of course very unlike the behavior of a break statement.<br><br>In the one-liner above I ignored the existence of parameters to break/continue, but the one-liner is also valid when parameters are supplied.  

---

Using continue and break:<br><br>

```
<?php
$stack = array('first', 'second', 'third', 'fourth', 'fifth');

foreach($stack AS $v){
    if($v == 'second')continue;
    if($v == 'fourth')break;
    echo $v.'<br>';
}
/*

first
third

*/

$stack2 = array('one'=>'first', 'two'=>'second', 'three'=>'third', 'four'=>'fourth', 'five'=>'fifth');
foreach($stack2 AS $k=>$v){
    if($v == 'second')continue;
    if($k == 'three')continue;
    if($v == 'fifth')break;
    echo $k.' ::: '.$v.'<br>';
}
/*

one ::: first
four ::: fourth

*/

?>
```
  

---

If you use a incrementing value in your loop, be sure to increment it before calling continue; or you might get an infinite loop.  

---

The most basic example that print "13", skipping over 2.<br><br>

```
<?php
$arr = array(1, 2, 3);
foreach($arr as $number) {
  if($number == 2) {
    continue;
  }
  print $number;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/control-structures.continue.php)

**[To root](/README.md)**