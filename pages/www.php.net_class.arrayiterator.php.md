# The ArrayIterator class



Another fine Iterator from php . You can use it especially when you have to iterate over objects<br><br>

```
<?php
$fruits = array(
    "apple" => "yummy",
    "orange" => "ah ya, nice",
    "grape" => "wow, I love it!",
    "plum" => "nah, not me"
);
$obj = new ArrayObject( $fruits );
$it = $obj->getIterator();

// How many items are we iterating over?

echo "Iterating over: " . $obj->count() . " values\n";

// Iterate over the values in the ArrayObject:
while( $it->valid() )
{
    echo $it->key() . "=" . $it->current() . "\n";
    $it->next();
}

// The good thing here is that it can be iterated with foreach loop

foreach ($it as $key=>$val)
echo $key.":".$val."\n";

/* Outputs something like */

Iterating over: 4 values
apple=yummy
orange=ah ya, nice
grape=wow, I love it!
plum=nah, not me

?>
```
<br><br>Regards.  

---

Need a callback on an iterated value, but don&apos;t have PHP 5.4+?  This makes is stupid easy:<br><br>

```
<?php
class ArrayCallbackIterator extends ArrayIterator {
  private $callback;
  public function __construct($value, $callback) {
    parent::__construct($value);
    $this->callback = $callback;
  }

  public function current() {
    $value = parent::current();
    return call_user_func($this->callback, $value);
  }
}
?>
```


You can use it pretty much exactly as the Array Iterator:



```
<?php
$iterator1 = new ArrayCallbackIterator($valueList, "callback_function");
$iterator2 = new ArrayCallbackIterator($valueList, array($object, "callback_class_method"));
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.arrayiterator.php)

**[To root](/README.md)**