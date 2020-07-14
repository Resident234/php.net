# switch



This is listed in the documentation above, but it&apos;s a bit tucked away between the paragraphs. The difference between a series of if statements and the switch statement is that the expression you&apos;re comparing with, is evaluated only once in a switch statement. I think this fact needs a little bit more attention, so here&apos;s an example:<br><br>

```
<?php
$a = 0;

if(++$a == 3) echo 3;
elseif(++$a == 2) echo 2;
elseif(++$a == 1) echo 1;
else echo "No match!";

// Outputs: 2

$a = 0;

switch(++$a) {
    case 3: echo 3; break;
    case 2: echo 2; break;
    case 1: echo 1; break;
    default: echo "No match!"; break;
}

// Outputs: 1
?>
```


It is therefore perfectly safe to do:



```
<?php
switch(winNobelPrizeStartingFromBirth()) {
case "peace": echo "You won the Nobel Peace Prize!"; break;
case "physics": echo "You won the Nobel Prize in Physics!"; break;
case "chemistry": echo "You won the Nobel Prize in Chemistry!"; break;
case "medicine": echo "You won the Nobel Prize in Medicine!"; break;
case "literature": echo "You won the Nobel Prize in Literature!"; break;
default: echo "You bought a rusty iron medal from a shady guy who insists it&apos;s a Nobel Prize..."; break;
}
?>
```
<br><br>without having to worry about the function being re-evaluated for every case. There&apos;s no need to preemptively save the result in a variable either.  

#

php 7.2.8.<br>The answer to the eternal question " what is faster?":<br>1 000 000 000 iterations.<br><br>

```
<?php
$s = time();
for ($i = 0; $i &lt; 1000000000; ++$i) {
  $x = $i%10;
  if ($x == 1) {
    $y = $x * 1;
  } elseif ($x == 2) {
    $y = $x * 2;
  } elseif ($x == 3) {
    $y = $x * 3;
  } elseif ($x == 4) {
    $y = $x * 4;
  } elseif ($x == 5) {
    $y = $x * 5;
  } elseif ($x == 6) {
    $y = $x * 6;
  } elseif ($x == 7) {
    $y = $x * 7;
  } elseif ($x == 8) {
    $y = $x * 8;
  } elseif ($x == 9) {
    $y = $x * 9;
  } else {
    $y = $x * 10;
  }
}
print("if: ".(time() - $s)."sec\n");
 
$s = time();
for ($i = 0; $i &lt; 1000000000; ++$i) {
  $x = $i%10;
  switch ($x) {
  case 1:
    $y = $x * 1;
    break;
  case 2:
    $y = $x * 2;
    break;
  case 3:
    $y = $x * 3;
    break;
  case 4:
    $y = $x * 4;
    break;
  case 5:
    $y = $x * 5;
    break;
  case 6:
    $y = $x * 6;
    break;
  case 7:
    $y = $x * 7;
    break;
  case 8:
    $y = $x * 8;
    break;
  case 9:
    $y = $x * 9;
    break;
  default:
    $y = $x * 10;
  }
}
print("switch: ".(time() - $s)."sec\n");
?>
```
<br><br>Results:<br>if: 69sec<br>switch: 42sec  

#

Just a trick I have picked up:<br><br>If you need to evaluate several variables to find the first one with an actual value, TRUE for instance. You can do it this was.<br><br>There is probably a better way but it has worked out well for me.<br><br>switch (true) {<br><br>  case (X != 1):<br><br>  case (Y != 1):<br><br>  default:<br>}  

#

In reply to lko at netuse dot de<br><br>Just so others know whom may not, that&apos;s because PHP does automatic type conversion if a string is evaluated as an integer (it sees the 2 in &apos;2string&apos; so when compared like if (&apos;2string&apos; == 2), PHP sees if (2 == 2) ).<br><br>I just tested it, but if you go:<br><br>

```
<?php

$string="2string";

switch($string)
{
    case (string) 1:
        echo "this is 1";
        break;
    case (string) 2:
        echo "this is 2";
        break;
    case &apos;2string&apos;:
        echo "this is a string";
        break;
}

?>
```
<br><br>The output will be "this is a string" and if you change $string to "2" it will again be "this is 2".<br><br>Just in case that may help anyone who may run into that problem.  

#

Attention if you have mixed types of value in one switch statemet it can make you some trouble<br><br>

```
<?php

$string="2string";

switch($string)
{
    case 1:
        echo "this is 1";
        break;
    case 2:
        echo "this is 2";
        break;
    case &apos;2string&apos;:
        echo "this is a string";
        break;
}

?>
```
<br><br>The swich-statement will halt on &apos;case 2&apos;<br><br>Answer: this is 2  

#

Remember, that you also could use functions in a switch.<br>For example, if you need to use regular expressions in a switch:<br><br>

```
<?php
$browserName = &apos;mozilla&apos;;
switch ($browserName) {
  case &apos;opera&apos;:
    echo &apos;opera&apos;;
  break;
  case (preg_match("/Mozilla( Firebird)?|phoenix/i", $browserName)?$browserName:!$browserName):
    echo "Mozilla or Mozilla Firebird";
  break;
  case &apos;konqueror&apos;:
    echo &apos;Konqueror&apos;;
  break;
  default:
    echo &apos;Default&apos;;
  break;
}
?>
```


or you could just use a regular expression for everything:



```
<?php
$uri = &apos;http://www.example.com&apos;;
switch (true) {
  case preg_match("/$http(s)?/i", $uri, $matches):
    echo $uri . &apos; is an http/https uri...&apos;;
  break;
  case preg_match("/$ftp(s)?/i", $uri, $matches):
    echo $uri . &apos; is an ftp/ftps uri...&apos;;
  break;
  default:
    echo &apos;default&apos;;
  break;
}
?>
```
  

#

Be careful if distinguishing between NULL and (int)0.  As implied in the above documentation, the case statements are equivalent to the &apos;==&apos; operator, not the &apos;===&apos; operator, so the following code did not work as i expected:<br><br>

```
<?php
$mixed = 0;
switch($mixed){
   case NULL: echo "NULL";  break;
   case 0: echo "zero";  break;
   default: echo "other"; break;
}
?>
```


Instead, I may use a chain of else-ifs.  (On this page, kriek at jonkreik dot com states that "in most cases [a switch statement] is 15% faster [than an else-if chain]" but jemore at m6net dotdot fr claims that when using ===, if/elseif/elseif can be 2 times faster than a switch().)

Alternatively, if i prefer the appearance of the switch() statement I may use a trick like the one nospam at please dot com presents:



```
<?php
$mixed = 0;
switch(TRUE){
   case (NULL===$mixed): //blah break;
   case (0   ===$mixed): //etc. break; 
}
?>
```
<br><br>code till dawn! mark meves!  

#

Something not mentioned in the documentation itself, and only touched on momentarily in these notes, is that the default: case need not be the last clause in the switch.<br>

```
<?php
for($i=0; $i&lt;8; ++$i)
{
    echo $i,"\t";
    switch($i)
    {
    case 1: echo "One"; break;
    case 2:
    default: echo "Thingy"; break;
    case 3:
    case 4: echo "Three or Four"; break;
    case 5: echo "Five"; break;
    }
    echo "\n";
}
?>
```

Outputs what you&apos;d expect, namely
0       Thingy
1       One
2       Thingy
3       Three or Four
4       Three or Four
5       Five
6       Thingy
7       Thingy
with case 2 and the default both producing the same result ("Thingy"); strictly speaking, the case 2 clause is completely empty and control just falls straight through. The same result could have been achieved with


```
<?php
switch($i)
{
    case 1: echo "One"; break;
    case 3:
    case 4: echo "Three or Four"; break;
    case 5: echo "Five"; break;
    default: echo "Thingy"; break;
}
?>
```
<br>But if "case 2" represented a fairly common case (other than "everything else"), then it would be better to declare it explicitly, not only because it saves time by not having to test EVERY other case first  (in the current example, PHP finds &apos;case 2&apos; in the first switch in two tests, but in the second switch it has to make four tests before giving up and going with the default) but also because someone (perhaps yourself in a few months&apos; time) will be reading the code and expecting to see it handled. Listing it explicitly aids comprehension  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.switch.php)

**[To root](/README.md)**