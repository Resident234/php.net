# switch





This is listed in the documentation above, but it&apos;s a bit tucked away between the paragraphs. The difference between a series of if statements and the switch statement is that the expression you&apos;re comparing with, is evaluated only once in a switch statement. I think this fact needs a little bit more attention, so here&apos;s an example:



```
<?php
$a = 0;

if(++$a == 3) echo 3;
elseif(++$a == 2) echo 2;
elseif(++$a == 1) echo 1;
else echo &quot;No match!&quot;;

// Outputs: 2

$a = 0;

switch(++$a) {
&#xA0; &#xA0; case 3: echo 3; break;
&#xA0; &#xA0; case 2: echo 2; break;
&#xA0; &#xA0; case 1: echo 1; break;
&#xA0; &#xA0; default: echo &quot;No match!&quot;; break;
}

// Outputs: 1
?>
```


It is therefore perfectly safe to do:



```
<?php
switch(winNobelPrizeStartingFromBirth()) {
case &quot;peace&quot;: echo &quot;You won the Nobel Peace Prize!&quot;; break;
case &quot;physics&quot;: echo &quot;You won the Nobel Prize in Physics!&quot;; break;
case &quot;chemistry&quot;: echo &quot;You won the Nobel Prize in Chemistry!&quot;; break;
case &quot;medicine&quot;: echo &quot;You won the Nobel Prize in Medicine!&quot;; break;
case &quot;literature&quot;: echo &quot;You won the Nobel Prize in Literature!&quot;; break;
default: echo &quot;You bought a rusty iron medal from a shady guy who insists it&apos;s a Nobel Prize...&quot;; break;
}
?>
```


without having to worry about the function being re-evaluated for every case. There&apos;s no need to preemptively save the result in a variable either.

  

#



php 7.2.8.
The answer to the eternal question &quot; what is faster?&quot;:
1 000 000 000 iterations.



```
<?php
$s = time();
for ($i = 0; $i &lt; 1000000000; ++$i) {
&#xA0; $x = $i%10;
&#xA0; if ($x == 1) {
&#xA0; &#xA0; $y = $x * 1;
&#xA0; } elseif ($x == 2) {
&#xA0; &#xA0; $y = $x * 2;
&#xA0; } elseif ($x == 3) {
&#xA0; &#xA0; $y = $x * 3;
&#xA0; } elseif ($x == 4) {
&#xA0; &#xA0; $y = $x * 4;
&#xA0; } elseif ($x == 5) {
&#xA0; &#xA0; $y = $x * 5;
&#xA0; } elseif ($x == 6) {
&#xA0; &#xA0; $y = $x * 6;
&#xA0; } elseif ($x == 7) {
&#xA0; &#xA0; $y = $x * 7;
&#xA0; } elseif ($x == 8) {
&#xA0; &#xA0; $y = $x * 8;
&#xA0; } elseif ($x == 9) {
&#xA0; &#xA0; $y = $x * 9;
&#xA0; } else {
&#xA0; &#xA0; $y = $x * 10;
&#xA0; }
}
print(&quot;if: &quot;.(time() - $s).&quot;sec\n&quot;);
 
$s = time();
for ($i = 0; $i &lt; 1000000000; ++$i) {
&#xA0; $x = $i%10;
&#xA0; switch ($x) {
&#xA0; case 1:
&#xA0; &#xA0; $y = $x * 1;
&#xA0; &#xA0; break;
&#xA0; case 2:
&#xA0; &#xA0; $y = $x * 2;
&#xA0; &#xA0; break;
&#xA0; case 3:
&#xA0; &#xA0; $y = $x * 3;
&#xA0; &#xA0; break;
&#xA0; case 4:
&#xA0; &#xA0; $y = $x * 4;
&#xA0; &#xA0; break;
&#xA0; case 5:
&#xA0; &#xA0; $y = $x * 5;
&#xA0; &#xA0; break;
&#xA0; case 6:
&#xA0; &#xA0; $y = $x * 6;
&#xA0; &#xA0; break;
&#xA0; case 7:
&#xA0; &#xA0; $y = $x * 7;
&#xA0; &#xA0; break;
&#xA0; case 8:
&#xA0; &#xA0; $y = $x * 8;
&#xA0; &#xA0; break;
&#xA0; case 9:
&#xA0; &#xA0; $y = $x * 9;
&#xA0; &#xA0; break;
&#xA0; default:
&#xA0; &#xA0; $y = $x * 10;
&#xA0; }
}
print(&quot;switch: &quot;.(time() - $s).&quot;sec\n&quot;);
?>
```


Results:
if: 69sec
switch: 42sec

  

#



Just a trick I have picked up:



If you need to evaluate several variables to find the first one with an actual value, TRUE for instance. You can do it this was.



There is probably a better way but it has worked out well for me.



switch (true) {



&#xA0; case (X != 1):



&#xA0; case (Y != 1):



&#xA0; default:

}

  

#



In reply to lko at netuse dot de

Just so others know whom may not, that&apos;s because PHP does automatic type conversion if a string is evaluated as an integer (it sees the 2 in &apos;2string&apos; so when compared like if (&apos;2string&apos; == 2), PHP sees if (2 == 2) ).

I just tested it, but if you go:



```
<?php

$string=&quot;2string&quot;;

switch($string)
{
&#xA0; &#xA0; case (string) 1:
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;this is 1&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case (string) 2:
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;this is 2&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case &apos;2string&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;this is a string&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
}

?>
```


The output will be &quot;this is a string&quot; and if you change $string to &quot;2&quot; it will again be &quot;this is 2&quot;.

Just in case that may help anyone who may run into that problem.

  

#



Attention if you have mixed types of value in one switch statemet it can make you some trouble



```
<?php

$string=&quot;2string&quot;;

switch($string)
{
&#xA0; &#xA0; case 1:
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;this is 1&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case 2:
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;this is 2&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case &apos;2string&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;this is a string&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
}

?>
```


The swich-statement will halt on &apos;case 2&apos;

Answer: this is 2

  

#



Remember, that you also could use functions in a switch.

For example, if you need to use regular expressions in a switch:





```
<?php

$browserName = &apos;mozilla&apos;;

switch ($browserName) {

&#xA0; case &apos;opera&apos;:

&#xA0; &#xA0; echo &apos;opera&apos;;

&#xA0; break;

&#xA0; case (preg_match(&quot;/Mozilla( Firebird)?|phoenix/i&quot;, $browserName)?$browserName:!$browserName):

&#xA0; &#xA0; echo &quot;Mozilla or Mozilla Firebird&quot;;

&#xA0; break;

&#xA0; case &apos;konqueror&apos;:

&#xA0; &#xA0; echo &apos;Konqueror&apos;;

&#xA0; break;

&#xA0; default:

&#xA0; &#xA0; echo &apos;Default&apos;;

&#xA0; break;

}

?>
```




or you could just use a regular expression for everything:





```
<?php

$uri = &apos;http://www.example.com&apos;;

switch (true) {

&#xA0; case preg_match(&quot;/$http(s)?/i&quot;, $uri, $matches):

&#xA0; &#xA0; echo $uri . &apos; is an http/https uri...&apos;;

&#xA0; break;

&#xA0; case preg_match(&quot;/$ftp(s)?/i&quot;, $uri, $matches):

&#xA0; &#xA0; echo $uri . &apos; is an ftp/ftps uri...&apos;;

&#xA0; break;

&#xA0; default:

&#xA0; &#xA0; echo &apos;default&apos;;

&#xA0; break;

}

?>
```



  

#



Be careful if distinguishing between NULL and (int)0.&#xA0; As implied in the above documentation, the case statements are equivalent to the &apos;==&apos; operator, not the &apos;===&apos; operator, so the following code did not work as i expected:



```
<?php
$mixed = 0;
switch($mixed){
&#xA0;&#xA0; case NULL: echo &quot;NULL&quot;;&#xA0; break;
&#xA0;&#xA0; case 0: echo &quot;zero&quot;;&#xA0; break;
&#xA0;&#xA0; default: echo &quot;other&quot;; break;
}
?>
```


Instead, I may use a chain of else-ifs.&#xA0; (On this page, kriek at jonkreik dot com states that &quot;in most cases [a switch statement] is 15% faster [than an else-if chain]&quot; but jemore at m6net dotdot fr claims that when using ===, if/elseif/elseif can be 2 times faster than a switch().)

Alternatively, if i prefer the appearance of the switch() statement I may use a trick like the one nospam at please dot com presents:



```
<?php
$mixed = 0;
switch(TRUE){
&#xA0;&#xA0; case (NULL===$mixed): //blah break;
&#xA0;&#xA0; case (0&#xA0;&#xA0; ===$mixed): //etc. break; 
}
?>
```


code till dawn! mark meves!

  

#



Something not mentioned in the documentation itself, and only touched on momentarily in these notes, is that the default: case need not be the last clause in the switch.


```
<?php
for($i=0; $i&lt;8; ++$i)
{
&#xA0; &#xA0; echo $i,&quot;\t&quot;;
&#xA0; &#xA0; switch($i)
&#xA0; &#xA0; {
&#xA0; &#xA0; case 1: echo &quot;One&quot;; break;
&#xA0; &#xA0; case 2:
&#xA0; &#xA0; default: echo &quot;Thingy&quot;; break;
&#xA0; &#xA0; case 3:
&#xA0; &#xA0; case 4: echo &quot;Three or Four&quot;; break;
&#xA0; &#xA0; case 5: echo &quot;Five&quot;; break;
&#xA0; &#xA0; }
&#xA0; &#xA0; echo &quot;\n&quot;;
}
?>
```

Outputs what you&apos;d expect, namely
0&#xA0; &#xA0; &#xA0;&#xA0; Thingy
1&#xA0; &#xA0; &#xA0;&#xA0; One
2&#xA0; &#xA0; &#xA0;&#xA0; Thingy
3&#xA0; &#xA0; &#xA0;&#xA0; Three or Four
4&#xA0; &#xA0; &#xA0;&#xA0; Three or Four
5&#xA0; &#xA0; &#xA0;&#xA0; Five
6&#xA0; &#xA0; &#xA0;&#xA0; Thingy
7&#xA0; &#xA0; &#xA0;&#xA0; Thingy
with case 2 and the default both producing the same result (&quot;Thingy&quot;); strictly speaking, the case 2 clause is completely empty and control just falls straight through. The same result could have been achieved with


```
<?php
switch($i)
{
&#xA0; &#xA0; case 1: echo &quot;One&quot;; break;
&#xA0; &#xA0; case 3:
&#xA0; &#xA0; case 4: echo &quot;Three or Four&quot;; break;
&#xA0; &#xA0; case 5: echo &quot;Five&quot;; break;
&#xA0; &#xA0; default: echo &quot;Thingy&quot;; break;
}
?>
```

But if &quot;case 2&quot; represented a fairly common case (other than &quot;everything else&quot;), then it would be better to declare it explicitly, not only because it saves time by not having to test EVERY other case first&#xA0; (in the current example, PHP finds &apos;case 2&apos; in the first switch in two tests, but in the second switch it has to make four tests before giving up and going with the default) but also because someone (perhaps yourself in a few months&apos; time) will be reading the code and expecting to see it handled. Listing it explicitly aids comprehension

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.switch.php)

**[To root](/README.md)**