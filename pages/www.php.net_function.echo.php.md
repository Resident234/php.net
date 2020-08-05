# echo



Passing multiple parameters to echo using commas (&apos;,&apos;)is not exactly identical to using the concatenation operator (&apos;.&apos;). There are two notable differences.<br><br>First, concatenation operators have much higher precedence. Referring to http://php.net/operators.precedence, there are many operators with lower precedence than concatenation, so it is a good idea to use the multi-argument form instead of passing concatenated strings.<br><br>

```
<?php
echo "The sum is " . 1 | 2; // output: "2". Parentheses needed.
echo "The sum is ", 1 | 2; // output: "The sum is 3". Fine.
?>
```


Second, a slightly confusing phenomenon is that unlike passing arguments to functions, the values are evaluated one by one.



```
<?php
function f($arg){
  var_dump($arg);
  return $arg;
}
echo "Foo" . f("bar") . "Foo";
echo "\n\n";
echo "Foo", f("bar"), "Foo";
?>
```


The output would be:
string(3) "bar"FoobarFoo

Foostring(3) "bar"
barFoo

It would become a confusing bug for a script that uses blocking functions like sleep() as parameters:



```
<?php
while(true){
  echo "Loop start!\n", sleep(1);
}
?>
```


vs



```
<?php
while(true){
  echo "Loop started!\n" . sleep(1);
}
?>
```
<br><br>With &apos;,&apos; the cursor stops at the beginning every newline, while with &apos;.&apos; the cursor stops after the 0 in the beginning every line (because sleep() returns 0).  

---

[Official documentation page](https://www.php.net/manual/en/function.echo.php)

**[To root](/README.md)**