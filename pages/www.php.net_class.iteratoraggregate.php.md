# The IteratorAggregate interface





Note that, at least as of 5.3, you still aren&apos;t allowed to return a normal Array from getIterator().

In some places, the docs wrap the array into an ArrayObject and return that.&#xA0; DON&apos;T DO IT.&#xA0; ArrayObject drops any empty-string keys on the floor when you iterate over it (again, at least as of 5.3).

Use ArrayIterator instead.&#xA0; I wouldn&apos;t be surprised if it didn&apos;t have its own set of wonderful bugs, but at the very least it works correctly when you use it with this method.

  

#



It might seem obvious, but you can return a compiled generator from your IteratorAggregate::getIterator() implementation.



```
<?php
class Collection implements IteratorAggregate
{
&#xA0; &#xA0; private $items = [];

&#xA0; &#xA0; public function __construct($items = [])
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;items = $items;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function getIterator()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return (function () {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while(list($key, $val) = each($this-&gt;items)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; yield $key =&gt; $val;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; })();
&#xA0; &#xA0; }
}

$data = [ &apos;A&apos;, &apos;B&apos;, &apos;C&apos;, &apos;D&apos; ];
$collection = new Collection($data);

foreach ($collection as $key =&gt; $val) {
&#xA0; &#xA0; echo sprintf(&quot;[%s] =&gt; %s\n&quot;, $key, $val);
}
?>
```



  

#





```
<?php
// IteratorAggregate
// Create indexed and associative arrays.

class myData implements IteratorAggregate {

&#xA0; &#xA0; private $array = [];
&#xA0; &#xA0; const TYPE_INDEXED = 1;
&#xA0; &#xA0; const TYPE_ASSOCIATIVE = 2;

&#xA0; &#xA0; public function __construct( array $data, $type = self::TYPE_INDEXED ) {
&#xA0; &#xA0; &#xA0; &#xA0; reset($data);
&#xA0; &#xA0; &#xA0; &#xA0; while( list($k, $v) = each($data) ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $type == self::TYPE_INDEXED ?
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;array[] = $v :
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;array[$k] = $v;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public function getIterator() {
&#xA0; &#xA0; &#xA0; &#xA0; return new ArrayIterator($this-&gt;array);
&#xA0; &#xA0; }

}

$obj = new myData([&apos;one&apos;=&gt;&apos;php&apos;,&apos;javascript&apos;,&apos;three&apos;=&gt;&apos;c#&apos;,&apos;java&apos;,], /*TYPE 1 or 2*/ );

foreach($obj as $key =&gt; $value) {
&#xA0; &#xA0; var_dump($key, $value);
&#xA0; &#xA0; echo PHP_EOL;
}

// if TYPE == 1
#int(0)
#string(3) &quot;php&quot;
#int(1)
#string(10) &quot;javascript&quot;
#int(2)
#string(2) &quot;c#&quot;
#int(3)
#string(4) &quot;java&quot;

// if TYPE == 2
#string(3) &quot;one&quot;
#string(3) &quot;php&quot;
#int(0)
#string(10) &quot;javascript&quot;
#string(5) &quot;three&quot;
#string(2) &quot;c#&quot;
#int(1)
#string(4) &quot;java&quot;
?>
```


Good luck!

  

#

[Official documentation page](https://www.php.net/manual/en/class.iteratoraggregate.php)

**[To root](/README.md)**