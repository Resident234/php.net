# BCMath Arbitrary Precision Mathematics



This extension is an interface to the GNU implementation as a library of the Basic Calculator utility by Philip Nelson; hence the name.  

#

Note that when you use implementation of factorial that ClaudiuS made, you get results even if you try to calculate factorial of number that you normally can&apos;t, e.g. 2.5, -2, etc. Here is safer implementation:<br>

```
<?php
/**
 * Calculates a factorial of given number.
 * @param string|int $num
 * @throws InvalidArgumentException
 * @return string
 */
function bcfact($num)
{
    if (!filter_var($num, FILTER_VALIDATE_INT) || $num <= 0) {
        throw new InvalidArgumentException(sprintf('Argument must be natural number, "%s" given.', $num));
    }

    for ($result = '1'; $num > 0; $num--) {
        $result = bcmul($result, $num);
    }

    return $result;
}
?>
```
  

#

Needed to compute some permutations and found the BC extension great but poor on functions, so untill this gets implemented here&apos;s the factorial function:<br><br>

```
<?php
/* BC FACTORIAL
 * n! = n * (n-1) * (n-2) .. 1 [eg. 5! = 5 * 4 * 3 * 2 * 1 = 120]
 */
function bcfact($n){
    $factorial=$n;
    while (--$n>1) $factorial=bcmul($factorial,$n);
    return $factorial;
}

print bcfact(50); 
//30414093201713378043612608166064768844377641568960512000000000000
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/book.bc.php)

**[To root](/README.md)**