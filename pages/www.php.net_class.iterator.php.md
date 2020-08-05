# The Iterator interface



Order of operations when using a foreach loop:<br><br>1. Before the first iteration of the loop, Iterator::rewind() is called.<br>2. Before each iteration of the loop, Iterator::valid() is called.<br>3a. It Iterator::valid() returns false, the loop is terminated.<br>3b. If Iterator::valid() returns true, Iterator::current() and<br>Iterator::key() are called.<br>4. The loop body is evaluated.<br>5. After each iteration of the loop, Iterator::next() is called and we repeat from step 2 above.<br><br>This is roughly equivalent to:<br><br>

```
<?php
$it->rewind();

while ($it->valid())
{
    $key = $it->key();
    $value = $it->current();

    // ...

    $it->next();
}
?>
```
<br><br>The loop isn&apos;t terminated until Iterator::valid() returns false or the body of the loop executes a break statement.<br><br>The only two methods that are always executed are Iterator::rewind() and Iterator::valid() (unless rewind throws an exception).<br><br>The Iterator::next() method need not return anything. It is defined as returning void. On the other hand, sometimes it is convenient for this method to return something, in which case you can do so if you want.<br><br>If your iterator is doing something expensive, like making a database query and iterating over the result set, the best place to make the query is probably in the Iterator::rewind() implementation.<br><br>In this case, the construction of the iterator itself can be cheap, and after construction you can continue to set the properties of the query all the way up to the beginning of the foreach loop since the<br>Iterator::rewind() method isn&apos;t called until then.<br><br>Things to keep in mind when making a database result set iterator:<br><br>* Make sure you close your cursor or otherwise clean up any previous query at the top of the rewind method. Otherwise your code will break if the same iterator is used in two consecutive foreach loops when the first loop terminates with a break statement before all the results are iterated over.<br><br>* Make sure your rewind() implementation tries to grab the first result so that the subsequent call to valid() will know whether or not the result set is empty. I do this by explicitly calling next() from the end of my rewind() implementation.<br><br>* For things like result set iterators, there really isn&apos;t always a "key" that you can return, unless you know you have a scalar primary key column in the query. Unfortunately, there will be cases where either the iterator doesn&apos;t know the primary key column because it isn&apos;t providing the query, the nature of the query is such that a primary key isn&apos;t applicable, the iterator is iterating over a table that doesn&apos;t have one, or the iterator is iterating over a table that has a compound primary key. In these cases, key() can return either:<br>the row index (based on a simple counter that you provide), or can simply return null.<br><br>Iterators can also be used to:<br><br>* iterate over the lines of a file or rows of a CSV file<br>* iterate over the characters of a string<br>* iterate over the tokens in an input stream<br>* iterate over the matches returned by an xpath expression<br>* iterate over the matches returned by a regexp<br>* iterate over the files in a folder<br>* etc...  

---



```
<?php
# - Here is an implementation of the Iterator interface for arrays
#     which works with maps (key/value pairs)
#     as well as traditional arrays
#     (contiguous monotonically increasing indexes).
#   Though it pretty much does what an array
#     would normally do within foreach() loops,
#     this class may be useful for using arrays
#     with code that generically/only supports the
#     Iterator interface.
#  Another use of this class is to simply provide
#     object methods with tightly controlling iteration of arrays.

class tIterator_array implements Iterator {
  private $myArray;

  public function __construct( $givenArray ) {
    $this->myArray = $givenArray;
  }
  function rewind() {
    return reset($this->myArray);
  }
  function current() {
    return current($this->myArray);
  }
  function key() {
    return key($this->myArray);
  }
  function next() {
    return next($this->myArray);
  }
  function valid() {
    return key($this->myArray) !== null;
  }
}

?>
```
  

---

If you have a custom iterator that may throw an exception in it&apos;s current() method, there is no way to catch the exception without breaking a foreach loop.<br><br>The following for loop allows you to skip elements for which $iterator-&gt;current() throws an exception, rather than breaking the loop.<br><br>

```
<?php
for ($iterator->rewind(); $iterator->valid(); $iterator->next()) {
    try {
        $value = $iterator->current();
    } catch (Exception $exception) {
        continue;
    }

    # ...
}
?>
```
  

---

It&apos;s important to note that following won&apos;t work if you have null values.<br><br>

```
<?php
    function valid() {
        var_dump(__METHOD__);
        return isset($this->array[$this->position]);
    }
?>
```


Other examples have shown the following which won't work if you have false values:



```
<?php
    function valid() {
        return $this->current() !== false;
    }
?>
```


Instead use:



```
<?php
    function valid() {
        return array_key_exists($this->array, $this->position);
    }
?>
```


Or the following if you do not store the position.



```
<?php
    public function valid() {
        return !is_null(key($this->array));
    }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.iterator.php)

**[To root](/README.md)**