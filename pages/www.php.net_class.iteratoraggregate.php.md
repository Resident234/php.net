# The IteratorAggregate interface



Note that, at least as of 5.3, you still aren&apos;t allowed to return a normal Array from getIterator().<br><br>In some places, the docs wrap the array into an ArrayObject and return that.  DON&apos;T DO IT.  ArrayObject drops any empty-string keys on the floor when you iterate over it (again, at least as of 5.3).<br><br>Use ArrayIterator instead.  I wouldn&apos;t be surprised if it didn&apos;t have its own set of wonderful bugs, but at the very least it works correctly when you use it with this method.  

---

It might seem obvious, but you can return a compiled generator from your IteratorAggregate::getIterator() implementation.<br><br>

```
<?php
class Collection implements IteratorAggregate
{
    private $items = [];

    public function __construct($items = [])
    {
        $this->items = $items;
    }

    public function getIterator()
    {
        return (function () {
            while(list($key, $val) = each($this->items)) {
                yield $key => $val;
            }
        })();
    }
}

$data = [ 'A', 'B', 'C', 'D' ];
$collection = new Collection($data);

foreach ($collection as $key => $val) {
    echo sprintf("[%s] => %s\n", $key, $val);
}
?>
```
  

---



```
<?php
// IteratorAggregate
// Create indexed and associative arrays.

class myData implements IteratorAggregate {

    private $array = [];
    const TYPE_INDEXED = 1;
    const TYPE_ASSOCIATIVE = 2;

    public function __construct( array $data, $type = self::TYPE_INDEXED ) {
        reset($data);
        while( list($k, $v) = each($data) ) {
            $type == self::TYPE_INDEXED ?
            $this->array[] = $v :
            $this->array[$k] = $v;
        }
    }

    public function getIterator() {
        return new ArrayIterator($this->array);
    }

}

$obj = new myData(['one'=>'php','javascript','three'=>'c#','java',], /*TYPE 1 or 2*/ );

foreach($obj as $key => $value) {
    var_dump($key, $value);
    echo PHP_EOL;
}

// if TYPE == 1
#int(0)
#string(3) "php"
#int(1)
#string(10) "javascript"
#int(2)
#string(2) "c#"
#int(3)
#string(4) "java"

// if TYPE == 2
#string(3) "one"
#string(3) "php"
#int(0)
#string(10) "javascript"
#string(5) "three"
#string(2) "c#"
#int(1)
#string(4) "java"
?>
```
<br><br>Good luck!  

---

[Official documentation page](https://www.php.net/manual/en/class.iteratoraggregate.php)

**[To root](/README.md)**