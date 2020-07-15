# Storing into an array (e.g. with iterator_to_array())



For example yield keyword with Fibonacci:<br><br>function getFibonacci()<br>{<br>    $i = 0;<br>    $k = 1; //first fibonacci value<br>    yield $k;<br>    while(true)<br>    {<br>        $k = $i + $k;<br>        $i = $k - $i;<br>        yield $k;        <br>    }<br>}<br><br>$y = 0;<br><br>foreach(getFibonacci() as $fibonacci)<br>{<br>    echo $fibonacci . "\n";<br>    $y++;    <br>    if($y &gt; 30)<br>    {<br>        break; // infinite loop prevent<br>    }<br>}  

#

[This comment replaces my previous comment]<br><br>You can use generators to do lazy loading of lists. You only compute the items that are actually used. However, when you want to load more items, how to cache the ones already loaded?<br><br>Here is how to do cached lazy loading with a generator:<br><br>

```
<?php
class CachedGenerator {
    protected $cache = [];
    protected $generator = null;

    public function __construct($generator) {
        $this->generator = $generator;
    }

    public function generator() {
        foreach($this->cache as $item) yield $item;

        while( $this->generator->valid() ) {
            $this->cache[] = $current = $this->generator->current();
            $this->generator->next();
            yield $current;
        }
    }
}
class Foobar {
    protected $loader = null;

    protected function loadItems() {
        foreach(range(0,10) as $i) {
            usleep(200000);
            yield $i;
        }
    }

    public function getItems() {
        $this->loader = $this->loader ?: new CachedGenerator($this->loadItems());
        return $this->loader->generator();
    }
}

$f = new Foobar;

# First
print "First\n";
foreach($f->getItems() as $i) {
    print $i . "\n";
    if( $i == 5 ) {
        break;
    }
}

# Second (items 1-5 are cached, 6-10 are loaded)
print "Second\n";
foreach($f->getItems() as $i) {
    print $i . "\n";
}

# Third (all items are cached and returned instantly)
print "Third\n";
foreach($f->getItems() as $i) {
    print $i . "\n";
}
?>
```
  

#

If for some strange reason you need a generator that doesn&apos;t yield anything, an empty function doesn&apos;t work; the function needs a yield statement to be recognised as a generator.<br><br>

```
<?php

function gndn()
{
}

foreach(gndn() as $it)
{
    echo 'FNORD';
}

?>
```


 But it's enough to have the yield syntactically present even if it's not reachable:



```
<?php

function gndn()
{
    if(false) { yield; }
}

foreach(gndn() as $it)
{
    echo 'FNORD';
}

?>
```
  

#

That is a simple fibonacci generator.<br><br>

```
<?php
function fibonacci($item) {
    $a = 0;
    $b = 1;
    for ($i = 0; $i < $item; $i++) {
        yield $a;
        $a = $b - $a;
        $b = $a + $b;
    }
}

# give me the first ten fibonacci numbers
$fibo = fibonacci(10);
foreach ($fibo as $value) {
    echo "$value\n";
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.generators.syntax.php)

**[To root](/README.md)**