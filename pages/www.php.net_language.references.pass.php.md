# Passing by Reference



By removing the ability to include the reference sign on function calls where pass-by-reference is incurred (I.e., function definition uses &amp;), the readability of the code suffers, as one has to look at the function definition to know if the variable being passed is by-ref or not (I.e., potential to be modified).  If both function calls and function definitions require the reference sign (I.e., &amp;), readability is improved, and it also lessens the potential of an inadvertent error in the code itself.  Going full on fatal error in 5.4.0 now forces everyone to have less readable code.  That is, does a function merely use the variable, or potentially modify it...now we have to find the function definition and physically look at it to know, whereas before we would know the intent immediately.  

---



```
<?php 
// PHP >= 5.6

// Here we use the 'use' operator to create a variable within the scope of the function. Although it may seem that the newly created variable has something to do with '$x' that is outside the function, we are actually creating a '$x' variable within the function that has nothing to do with the '$x' variable outside the function. We are talking about the same names but different content locations in memory.
$x = 10;
(function() use ($x){
    $x = $x*$x;
    var_dump($x); // 100
})();
var_dump($x); // 10

// Now the magic happens with using the reference (&amp;). Now we are actually accessing the contents of the '$y' variable that is outside the scope of the function. All the actions that we perform with the variable '$y' within the function will be reflected outside the scope of this same function. Remembering this would be an impure function in the functional paradigm, since we are changing the value of a variable by reference.
$y = 10;
(function() use (&amp;$y){
    $y = $y*$y;
    var_dump($y); // 100
})();
var_dump($y); // 100
?>
```
  

---

beware unset()  destroys references<br><br>$x = &apos;x&apos;;<br>change( $x );<br>echo $x; // outputs "x" not "q23"  ---- remove the unset() and output is "q23" not "x"<br><br>function change( &amp; $x )<br>{<br>    unset( $x );<br>    $x = &apos;q23&apos;;<br>    return true;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/language.references.pass.php)

**[To root](/README.md)**