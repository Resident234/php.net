# array_push



If you&apos;re going to use array_push() to insert a "$key" =&gt; "$value" pair into an array, it can be done using the following:<br><br>    $data[$key] = $value;<br><br>It is not necessary to use array_push.  

---

I&apos;ve done a small comparison between array_push() and the $array[] method and the $array[] seems to be a lot faster.<br><br>

```
<?php
$array = array();
for ($x = 1; $x <= 100000; $x++)
{
    $array[] = $x;
}
?>
```

takes 0.0622200965881 seconds

and



```
<?php
$array = array();
for ($x = 1; $x <= 100000; $x++)
{
    array_push($array, $x);
}
?>
```
<br>takes 1.63195490837 seconds<br><br>so if your not making use of the return value of array_push() its better to use the $array[] way.<br><br>Hope this helps someone.  

---

Rodrigo de Aquino asserted that instead of using array_push to append to an associative array you can instead just do...<br><br>        $data[$key] = $value;<br><br>...but this is actually not true. Unlike array_push and even...<br><br>        $data[] = $value;<br><br>...Rodrigo&apos;s suggestion is NOT guaranteed to append the new element to the END of the array. For instance...<br><br>        $data[&apos;one&apos;] = 1;<br>        $data[&apos;two&apos;] = 2;<br>        $data[&apos;three&apos;] = 3;<br>        $data[&apos;four&apos;] = 4;<br> <br>...might very well result in an array that looks like this...<br><br>       [ "four" =&gt; 4, "one" =&gt; 1, "three" =&gt; 3, "two" =&gt; 2 ]<br><br>I can only assume that PHP sorts the array as elements are added to make it easier for it to find a specified element by its key later. In many cases it won&apos;t matter if the array is not stored internally in the same order you added the elements, but if, for instance, you execute a foreach on the array later, the elements may not be processed in the order you need them to be.<br><br>If you want to add elements to the END of an associative array you should use the unary array union operator (+=) instead...<br><br>       $data[&apos;one&apos;] = 1;<br>       $data += [ "two" =&gt; 2 ];<br>       $data += [ "three" =&gt; 3 ];<br>       $data += [ "four" =&gt; 4 ];<br><br>You can also, of course, append more than one element at once...<br><br>       $data[&apos;one&apos;] = 1;<br>       $data += [ "two" =&gt; 2, "three" =&gt; 3 ];<br>       $data += [ "four" =&gt; 4 ];<br><br>Note that like array_push (but unlike $array[] =) the array must exist before the unary union, which means that if you are building an array in a loop you need to declare an empty array first...<br><br>       $data = [];<br>       for ( $i = 1; $i &lt; 5; $i++ ) {<br>              $data += [ "element$i" =&gt; $i ];<br>       }<br><br>...which will result in an array that looks like this...<br><br>      [ "element1" =&gt; 1, "element2" =&gt; 2, "element3" =&gt; 3, "element4" =&gt; 4 ]  

---

There is a mistake in the note by egingell at sisna dot com 12 years ago. The tow dimensional array will output "d,e,f", not "a,b,c".<br><br>

```
<?php
$stack = array('a', 'b', 'c');
array_push($stack, array('d', 'e', 'f'));
print_r($stack);
?>
```
<br><br>The above will output this:<br>Array (<br>  [0] =&gt; a<br>  [1] =&gt; b<br>  [2] =&gt; c<br>  [3] =&gt; Array (<br>     [0] =&gt; d<br>     [1] =&gt; e<br>     [2] =&gt; f<br>  )<br>)  

---

If you&apos;re adding multiple values to an array in a loop, it&apos;s faster to use array_push than repeated [] = statements that I see all the time:<br><br>

```
<?php
class timer
{
        private $start;
        private $end;

        public function timer()
        {
                $this->start = microtime(true);
        }

        public function Finish()
        {
                $this->end = microtime(true);
        }

        private function GetStart()
        {
                if (isset($this->start))
                        return $this->start;
                else
                        return false;
        }

        private function GetEnd()
        {
                if (isset($this->end))
                        return $this->end;
                else
                        return false;
        }

        public function GetDiff()
        {
                return $this->GetEnd() - $this->GetStart();
        }

        public function Reset()
        {
                $this->start = microtime(true);
        }

}

echo "Adding 100k elements to array with []\n\n";
$ta = array();
$test = new Timer();
for ($i = 0; $i < 100000; $i++)
{
        $ta[] = $i;
}
$test->Finish();
echo $test->GetDiff();

echo "\n\nAdding 100k elements to array with array_push\n\n";
$test->Reset();
for ($i = 0; $i < 100000; $i++)
{
        array_push($ta,$i);
}
$test->Finish();
echo $test->GetDiff();

echo "\n\nAdding 100k elements to array with [] 10 per iteration\n\n";
$test->Reset();
for ($i = 0; $i < 10000; $i++)
{
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
        $ta[] = $i;
}
$test->Finish();
echo $test->GetDiff();

echo "\n\nAdding 100k elements to array with array_push 10 per iteration\n\n";
$test->Reset();
for ($i = 0; $i < 10000; $i++)
{
        array_push($ta,$i,$i,$i,$i,$i,$i,$i,$i,$i,$i);
}
$test->Finish();
echo $test->GetDiff();
?>
```
<br><br>Output<br><br>$ php5 arraypush.php<br>X-Powered-By: PHP/5.2.5<br>Content-type: text/html<br><br>Adding 100k elements to array with []<br><br>0.044686794281006<br><br>Adding 100k elements to array with array_push<br><br>0.072616100311279<br><br>Adding 100k elements to array with [] 10 per iteration<br><br>0.034690141677856<br><br>Adding 100k elements to array with array_push 10 per iteration<br><br>0.023932933807373  

---

If you push an array onto the stack, PHP will add the whole array to the next element instead of adding the keys and values to the array. If this is not what you want, you&apos;re better off using array_merge() or traverse the array you&apos;re pushing on and add each element with $stack[$key] = $value.<br><br>

```
<?php

$stack = array('a', 'b', 'c');
array_push($stack, array('d', 'e', 'f'));
print_r($stack);

?>
```
<br>The above will output this:<br>Array (<br>  [0] =&gt; a<br>  [1] =&gt; b<br>  [2] =&gt; c<br>  [3] =&gt; Array (<br>     [0] =&gt; a<br>     [1] =&gt; b<br>     [2] =&gt; c<br>  )<br>)  

---

[Official documentation page](https://www.php.net/manual/en/function.array-push.php)

**[To root](/README.md)**