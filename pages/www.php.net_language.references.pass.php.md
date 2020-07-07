# Passing by Reference





By removing the ability to include the reference sign on function calls where pass-by-reference is incurred (I.e., function definition uses &amp;), the readability of the code suffers, as one has to look at the function definition to know if the variable being passed is by-ref or not (I.e., potential to be modified).&#xA0; If both function calls and function definitions require the reference sign (I.e., &amp;), readability is improved, and it also lessens the potential of an inadvertent error in the code itself.&#xA0; Going full on fatal error in 5.4.0 now forces everyone to have less readable code.&#xA0; That is, does a function merely use the variable, or potentially modify it...now we have to find the function definition and physically look at it to know, whereas before we would know the intent immediately.

  

#





```
<?php 
// PHP &gt;= 5.6

// Here we use the &apos;use&apos; operator to create a variable within the scope of the function. Although it may seem that the newly created variable has something to do with &apos;$x&apos; that is outside the function, we are actually creating a &apos;$x&apos; variable within the function that has nothing to do with the &apos;$x&apos; variable outside the function. We are talking about the same names but different content locations in memory.
$x = 10;
(function() use ($x){
&#xA0; &#xA0; $x = $x*$x;
&#xA0; &#xA0; var_dump($x); // 100
})();
var_dump($x); // 10

// Now the magic happens with using the reference (&amp;). Now we are actually accessing the contents of the &apos;$y&apos; variable that is outside the scope of the function. All the actions that we perform with the variable &apos;$y&apos; within the function will be reflected outside the scope of this same function. Remembering this would be an impure function in the functional paradigm, since we are changing the value of a variable by reference.
$y = 10;
(function() use (&amp;$y){
&#xA0; &#xA0; $y = $y*$y;
&#xA0; &#xA0; var_dump($y); // 100
})();
var_dump($y); // 100
?>
```



  

#



beware unset()&#xA0; destroys references

$x = &apos;x&apos;;
change( $x );
echo $x; // outputs &quot;x&quot; not &quot;q23&quot;&#xA0; ---- remove the unset() and output is &quot;q23&quot; not &quot;x&quot;

function change( &amp; $x )
{
&#xA0; &#xA0; unset( $x );
&#xA0; &#xA0; $x = &apos;q23&apos;;
&#xA0; &#xA0; return true;
}

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.pass.php)

**[To root](/README.md)**