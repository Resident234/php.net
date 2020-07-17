# array_rand



Note: array_rand uses the libc generator, which is slower and less-random than Mersenne Twister.<br><br>

```
<?php
    $a = ['http://php.net/', 'http://google.com/', 'http://bbc.co.uk/'];

    $website = $a[mt_rand(0, count($a) - 1)];
?>
```
<br><br>This is a better alternative.  

#

If the array elements are unique, and are all integers or strings, here is a simple way to pick $n random *values* (not keys) from an array $array:<br><br>

```
<?php array_rand(array_flip($array), $n); ?>
```
  

#

Looks like this function has a strange randomness.<br><br>If you take any number of elements in an array which has 40..100 elements, the 31st one is always by far the less occuring (by about 10% less than others).<br><br>I tried this piece of code at home (PHP Version 5.3.2-1ubuntu4.9) and on my server (PHP Version 5.2.17), unfortunately i haven&apos;t any server with the last version here :<br><br>

```
<?php
$valeurs = range(1, 40);
$proba = array_fill(1, 40, 0);
for ($i = 0; $i < 10000; ++$i)
{
    $tirage_tab = array_rand($valeurs, 10);
    foreach($tirage_tab as $key => $value)
    {
        $proba[$valeurs[$value]]++;
    }
}

asort($proba);
echo "Proba : <br/>\n";
foreach($proba as $key => $value)
{
    echo "$key : $value<br/>\n";
}
?>
```
<br><br>In every try, the number of occurrences change a bit but the 31 is always far less (around 2200) than the others (2400-2600). I tried with 50 and 100 elements, no change. I tried with more or less elements to pick (second parameter to array_rand), same result. If you pick only one element it&apos;s even worse : 31 has half the result of the others.<br><br>For this particular case, i recommend shuffling the array and taking the nth first elements, in this test it&apos;s 60% faster and the statistics are ok.  

#



```
<?php
// An example how to fetch multiple values from array_rand
$a = [ 'a', 'b', 'c', 'd', 'e', 'f', 'g' ];
$n = 3;

// If you want to fetch multiple values you can try this:
print_r( array_intersect_key( $a, array_flip( array_rand( $a, $n ) ) ) );

// If you want to re-index keys wrap the call in 'array_values':
print_r( array_values( array_intersect_key( $a, array_flip( array_rand( $a, $n ) ) ) ) );?>
```
  

#

An example for getting random value from arrays;<br><br>

```
<?php
function array_random($arr, $num = 1) {
    shuffle($arr);
    
    $r = array();
    for ($i = 0; $i < $num; $i++) {
        $r[] = $arr[$i];
    }
    return $num == 1 ? $r[0] : $r;
}

$a = array("apple", "banana", "cherry");
print_r(array_random($a));
print_r(array_random($a, 2));
?>
```


cherry
Array
(
    [0] => banana
    [1] => apple
)

And example for getting random value from assoc arrays;



```
<?php
function array_random_assoc($arr, $num = 1) {
    $keys = array_keys($arr);
    shuffle($keys);
    
    $r = array();
    for ($i = 0; $i < $num; $i++) {
        $r[$keys[$i]] = $arr[$keys[$i]];
    }
    return $r;
}

$a = array("a" => "apple", "b" => "banana", "c" => "cherry");
print_r(array_random_assoc($a));
print_r(array_random_assoc($a, 2));
?>
```
<br><br>Array<br>(<br>    [c] =&gt; cherry<br>)<br>Array<br>(<br>    [a] =&gt; apple<br>    [b] =&gt; banana<br>)  

#



```
<?php

/**
 * Wraps array_rand call with additional checks
 *
 * TLDR; not so radom as you'd wish.
 *
 * NOTICE: the closer you get to the input arrays length, for the n parameter, the  output gets less random.
 * e.g.: array_random($a, count($a)) == $a will yield true
 * This, most certainly, has to do with the method used for making the array random (see other comments).
 *
 * @throws OutOfBoundsException &#x2013; if n less than one or exceeds size of input array
 *
 * @param array $array &#x2013; array to randomize
 * @param int $n &#x2013; how many elements to return
 * @return array
 */
function array_random(array $array, int $n = 1): array
{
    if ($n < 1 || $n > count($array)) {
        throw new OutOfBoundsException();
    }

    return ($n !== 1)
        ? array_values(array_intersect_key($array, array_flip(array_rand($array, $n))))
        : array($array[array_rand($array)]);
}?>
```
  

#

I agree with Sebmil (http://php.net/manual/en/function.array-rand.php#105265) that "array_rand()" produces weird and very uneven random distribution (as of my local PHP 5.3.8 and my public host&apos;s PHP 5.2.17).<br>Unfortunately, I haven&apos;t got any access either to a server with the latest PHP version. My info is for those of you who like to check things for themselves and who don&apos;t believe all of the official statements in the docs.<br>I&apos;ve made a simple adjustment of his test code like this:<br>

```
<?php 
$s=1;    // Start value
$c=50;    // Count / End value
$test=array_fill($s, $c, 0);
$ts=microtime(true);
for($i=0; $i<5000000; $i++){
    $idx=mt_rand($s, $c);    // Try it with rand() - simpler but more evenly distributed than mt_rand()
    $test[$idx]++;
}
$te=microtime(true);
$te=($te-$ts)*1000.0;    // Loop time in miliseconds

asort($test);
echo "Test mt_rand() in ".$te." ms: <br/>\n";
foreach($test as $k=>$v) echo "$k :\t$v <br/>\n";
?>
```
<br><br>And it appears to me that simple "$idx=rand(0, count($test)-1);" is much better than "$idx=array_rand($test, 1);".<br>And what&apos;s more the simpler and a bit slower (0 ms up to total 712.357 ms at 5 mln cycles) "rand()" is better than "mt_rand()" in simple everyday use cases because it is more evenly distributed (difference least vs. most often numbers: ca. 0.20-1.28 % for "rand()" vs. ca. 1.43-1.68 % for "mt_rand()").<br>Try it for yourself... although it depends on your software and hardware configuration, range of numbers to choose from (due to random patterns), number of cycles in the loop, and temporary (public) server load as well.  

#

It doesn&apos;t explicitly say it in the documentation, but PHP won&apos;t pick the same key twice in one call.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-rand.php)

**[To root](/README.md)**