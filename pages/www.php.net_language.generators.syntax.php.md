# Storing into an array (e.g. with iterator_to_array())





For example yield keyword with Fibonacci:

function getFibonacci()
{
&#xA0; &#xA0; $i = 0;
&#xA0; &#xA0; $k = 1; //first fibonacci value
&#xA0; &#xA0; yield $k;
&#xA0; &#xA0; while(true)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $k = $i + $k;
&#xA0; &#xA0; &#xA0; &#xA0; $i = $k - $i;
&#xA0; &#xA0; &#xA0; &#xA0; yield $k;&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }
}

$y = 0;

foreach(getFibonacci() as $fibonacci)
{
&#xA0; &#xA0; echo $fibonacci . &quot;\n&quot;;
&#xA0; &#xA0; $y++;&#xA0; &#xA0; 
&#xA0; &#xA0; if($y &gt; 30)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; break; // infinite loop prevent
&#xA0; &#xA0; }
}

  

#



[This comment replaces my previous comment]

You can use generators to do lazy loading of lists. You only compute the items that are actually used. However, when you want to load more items, how to cache the ones already loaded?

Here is how to do cached lazy loading with a generator:



```
<?php
class CachedGenerator {
&#xA0; &#xA0; protected $cache = [];
&#xA0; &#xA0; protected $generator = null;

&#xA0; &#xA0; public function __construct($generator) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;generator = $generator;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function generator() {
&#xA0; &#xA0; &#xA0; &#xA0; foreach($this-&gt;cache as $item) yield $item;

&#xA0; &#xA0; &#xA0; &#xA0; while( $this-&gt;generator-&gt;valid() ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;cache[] = $current = $this-&gt;generator-&gt;current();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;generator-&gt;next();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; yield $current;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}
class Foobar {
&#xA0; &#xA0; protected $loader = null;

&#xA0; &#xA0; protected function loadItems() {
&#xA0; &#xA0; &#xA0; &#xA0; foreach(range(0,10) as $i) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; usleep(200000);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; yield $i;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public function getItems() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;loader = $this-&gt;loader ?: new CachedGenerator($this-&gt;loadItems());
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;loader-&gt;generator();
&#xA0; &#xA0; }
}

$f = new Foobar;

# First
print &quot;First\n&quot;;
foreach($f-&gt;getItems() as $i) {
&#xA0; &#xA0; print $i . &quot;\n&quot;;
&#xA0; &#xA0; if( $i == 5 ) {
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }
}

# Second (items 1-5 are cached, 6-10 are loaded)
print &quot;Second\n&quot;;
foreach($f-&gt;getItems() as $i) {
&#xA0; &#xA0; print $i . &quot;\n&quot;;
}

# Third (all items are cached and returned instantly)
print &quot;Third\n&quot;;
foreach($f-&gt;getItems() as $i) {
&#xA0; &#xA0; print $i . &quot;\n&quot;;
}
?>
```



  

#



If for some strange reason you need a generator that doesn&apos;t yield anything, an empty function doesn&apos;t work; the function needs a yield statement to be recognised as a generator.



```
<?php

function gndn()
{
}

foreach(gndn() as $it)
{
&#xA0; &#xA0; echo &apos;FNORD&apos;;
}

?>
```


 But it&apos;s enough to have the yield syntactically present even if it&apos;s not reachable:



```
<?php

function gndn()
{
&#xA0; &#xA0; if(false) { yield; }
}

foreach(gndn() as $it)
{
&#xA0; &#xA0; echo &apos;FNORD&apos;;
}

?>
```



  

#



That is a simple fibonacci generator.



```
<?php
function fibonacci($item) {
&#xA0; &#xA0; $a = 0;
&#xA0; &#xA0; $b = 1;
&#xA0; &#xA0; for ($i = 0; $i &lt; $item; $i++) {
&#xA0; &#xA0; &#xA0; &#xA0; yield $a;
&#xA0; &#xA0; &#xA0; &#xA0; $a = $b - $a;
&#xA0; &#xA0; &#xA0; &#xA0; $b = $a + $b;
&#xA0; &#xA0; }
}

# give me the first ten fibonacci numbers
$fibo = fibonacci(10);
foreach ($fibo as $value) {
&#xA0; &#xA0; echo &quot;$value\n&quot;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.generators.syntax.php)

**[To root](/README.md)**