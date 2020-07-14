# call_user_func



I benchmarked the comparison in speed between variable functions, call_user_func, and eval.  My results are below:<br><br>Variable functions took 0.125958204269 seconds.<br>call_user_func took 0.485446929932 seconds.<br>eval took 2.78526711464 seconds.<br><br>This was run on a Compaq Proliant server, 180MHz Pentium Pro 256MB RAM.  Code is as follows:<br><br>

```
<?php

function fa () { return 1; }
function fb () { return 1; }
function fc () { return 1; }

$calla = &apos;fa&apos;;
$callb = &apos;fb&apos;;
$callc = &apos;fc&apos;;

$time = microtime( true );
for( $i = 5000; $i--; ) {
    $x = 0;
    $x += $calla();
    $x += $callb();
    $x += $callc();
    if( $x != 3 ) die( &apos;Bad numbers&apos; );
}
echo( "Variable functions took " . (microtime( true ) - $time) . " seconds.&lt;br /&gt;" );

$time = microtime( true );
for( $i = 5000; $i--; ) {
    $x = 0;
    $x += call_user_func(&apos;fa&apos;, &apos;&apos;);
    $x += call_user_func(&apos;fb&apos;, &apos;&apos;);
    $x += call_user_func(&apos;fc&apos;, &apos;&apos;);
    if( $x != 3 ) die( &apos;Bad numbers&apos; );
}
echo( "call_user_func took " . (microtime( true ) - $time) . " seconds.&lt;br /&gt;" );

$time = microtime( true );
for( $i = 5000; $i--; ) {
    $x = 0;
    eval( &apos;$x += &apos; . $calla . &apos;();&apos; );
    eval( &apos;$x += &apos; . $callb . &apos;();&apos; );
    eval( &apos;$x += &apos; . $callc . &apos;();&apos; );
    if( $x != 3 ) die( &apos;Bad numbers&apos; );
}
echo( "eval took " . (microtime( true ) - $time) . " seconds.&lt;br /&gt;" );

?>
```
  

#

if you simply want to dynamically call a method on an object it is not necessary to use call_user_function but instead you can do the following:<br><br>

```
<?php

$method_name = "AMethodName";

$obj = new ClassName();

$obj-&gt;{$method_name}();

?>
```
<br><br>I&apos;ve used the above so I know it works.<br><br>Regards,<br>-- Greg  

#

@insta at citiesunlimited dot com &amp; @Maresa<br><br>call_user_func() alleged slowness is quite over estimated when it comes to real use cases. we are talking about loosing fraction of a second every million calls, which by the way would take less than half a sec to execute in the worst case.<br><br>I don&apos;t know of many processes that would actually suffer from this kind of overhead.<br><br>Iterations: 100 000<br>Averaged over: 10<br>PHP 5.6.30 (cli) (built: Jan 18 2017 19:47:28)<br>Overall Average<br>+------------------------+----------+-----------+--------+<br>| Invocation             | Time (s) | Delta (s) | %      |<br>+------------------------+----------+-----------+--------+<br>| directFunction         | 0.0089   | -0.0211   | -70.19 |<br>| directStatic           | 0.0098   | -0.0202   | -67.39 |<br>| directLambda           | 0.0109   | -0.0191   | -63.52 |<br>| directInstance         | 0.0116   | -0.0184   | -61.31 |<br>| directClosure          | 0.0150   | -0.0150   | -50.15 |<br>| Invoke                 | 0.0282   | -0.0018   | -6.13  |<br>| call_user_func         | 0.0300   |           |        |<br>| ClosureFactory         | 0.0316   | +0.0016   | +5.20  |<br>| assignedClosureFactory | 0.0328   | +0.0028   | +9.28  |<br>| call_user_func_array   | 0.0399   | +0.0099   | +33.02 |<br>| InvokeCallUserFunc     | 0.0418   | +0.0118   | +39.17 |<br>| directImplementation   | 0.0475   | +0.0175   | +58.28 |<br>+------------------------+----------+-----------+--------+<br><br>Iterations: 100 000<br>Averaged over: 10<br>PHP 7.1.2 (cli) (built: Feb 14 2017 21:24:45) <br>Overall Average<br>+------------------------+----------+-----------+--------+<br>| Invocation             | Time (s) | Delta (s) | %      |<br>+------------------------+----------+-----------+--------+<br>| directFunction         | 0.0043   | -0.0096   | -68.92 |<br>| directStatic           | 0.0050   | -0.0089   | -64.04 |<br>| directInstance         | 0.0058   | -0.0081   | -58.22 |<br>| directLambda           | 0.0063   | -0.0075   | -54.44 |<br>| directClosure          | 0.0081   | -0.0058   | -41.57 |<br>| call_user_func         | 0.0139   |           |        |<br>| call_user_func_array   | 0.0147   | +0.0008   | +5.84  |<br>| Invoke                 | 0.0187   | +0.0048   | +34.61 |<br>| ClosureFactory         | 0.0207   | +0.0069   | +49.43 |<br>| assignedClosureFactory | 0.0219   | +0.0080   | +57.75 |<br>| directImplementation   | 0.0232   | +0.0094   | +67.53 |<br>| InvokeCallUserFunc     | 0.0264   | +0.0126   | +90.67 |<br>+------------------------+----------+-----------+--------+<br><br>If you want more details : https://github.com/fab2s/call_user_func  

#

A good use for call_user_func(); is for recursive functions.<br>If you&apos;re distributing code, you will often come across users who will rename functions and break the code..<br>Use this: call_user_func(__FUNCTION__, ... ); inside a function to call itself with whatever parameters you want.<br><br>

```
<?php
// example, an extremely simplified factorial calculator..
// it&apos;s quite obvious when someone renames the function, it&apos;ll spit out an error because it wants to call itself.
function Factorial($i=1) {
  return($i==1?1:$i*Factorial($i-1));
}

// you can give this function whatever name you want, it&apos;ll always work, of course if you initially call it using the name you gave it.
function qwertyuiop($i=1) {
  return($i==1?1:$i*call_user_func(__FUNCTION__,$i-1));
}
?>
```
<br><br>Just that I didn&apos;t see any reference to recursive functions when user_call_func(); really helps.  

#

You don&apos;t need to use this function to call a variable class function. Instead you can do the following:<br><br>$this-&gt;{$fnname}();<br><br>The example works in PHP 5 from within the class. It is the {} that do the trick.<br><br>Regards,<br>Julian.  

#

[Official documentation page](https://www.php.net/manual/en/function.call-user-func.php)

**[To root](/README.md)**