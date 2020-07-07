# The Iterator interface





Order of operations when using a foreach loop:

1. Before the first iteration of the loop, Iterator::rewind() is called.
2. Before each iteration of the loop, Iterator::valid() is called.
3a. It Iterator::valid() returns false, the loop is terminated.
3b. If Iterator::valid() returns true, Iterator::current() and
Iterator::key() are called.
4. The loop body is evaluated.
5. After each iteration of the loop, Iterator::next() is called and we repeat from step 2 above.

This is roughly equivalent to:



```
<?php
$it-&gt;rewind();

while ($it-&gt;valid())
{
&#xA0; &#xA0; $key = $it-&gt;key();
&#xA0; &#xA0; $value = $it-&gt;current();

&#xA0; &#xA0; // ...

&#xA0; &#xA0; $it-&gt;next();
}
?>
```


The loop isn&apos;t terminated until Iterator::valid() returns false or the body of the loop executes a break statement.

The only two methods that are always executed are Iterator::rewind() and Iterator::valid() (unless rewind throws an exception).

The Iterator::next() method need not return anything. It is defined as returning void. On the other hand, sometimes it is convenient for this method to return something, in which case you can do so if you want.

If your iterator is doing something expensive, like making a database query and iterating over the result set, the best place to make the query is probably in the Iterator::rewind() implementation.

In this case, the construction of the iterator itself can be cheap, and after construction you can continue to set the properties of the query all the way up to the beginning of the foreach loop since the
Iterator::rewind() method isn&apos;t called until then.

Things to keep in mind when making a database result set iterator:

* Make sure you close your cursor or otherwise clean up any previous query at the top of the rewind method. Otherwise your code will break if the same iterator is used in two consecutive foreach loops when the first loop terminates with a break statement before all the results are iterated over.

* Make sure your rewind() implementation tries to grab the first result so that the subsequent call to valid() will know whether or not the result set is empty. I do this by explicitly calling next() from the end of my rewind() implementation.

* For things like result set iterators, there really isn&apos;t always a &quot;key&quot; that you can return, unless you know you have a scalar primary key column in the query. Unfortunately, there will be cases where either the iterator doesn&apos;t know the primary key column because it isn&apos;t providing the query, the nature of the query is such that a primary key isn&apos;t applicable, the iterator is iterating over a table that doesn&apos;t have one, or the iterator is iterating over a table that has a compound primary key. In these cases, key() can return either:
the row index (based on a simple counter that you provide), or can simply return null.

Iterators can also be used to:

* iterate over the lines of a file or rows of a CSV file
* iterate over the characters of a string
* iterate over the tokens in an input stream
* iterate over the matches returned by an xpath expression
* iterate over the matches returned by a regexp
* iterate over the files in a folder
* etc...

  

#





```
<?php
# - Here is an implementation of the Iterator interface for arrays
#&#xA0; &#xA0;&#xA0; which works with maps (key/value pairs)
#&#xA0; &#xA0;&#xA0; as well as traditional arrays
#&#xA0; &#xA0;&#xA0; (contiguous monotonically increasing indexes).
#&#xA0;&#xA0; Though it pretty much does what an array
#&#xA0; &#xA0;&#xA0; would normally do within foreach() loops,
#&#xA0; &#xA0;&#xA0; this class may be useful for using arrays
#&#xA0; &#xA0;&#xA0; with code that generically/only supports the
#&#xA0; &#xA0;&#xA0; Iterator interface.
#&#xA0; Another use of this class is to simply provide
#&#xA0; &#xA0;&#xA0; object methods with tightly controlling iteration of arrays.

class tIterator_array implements Iterator {
&#xA0; private $myArray;

&#xA0; public function __construct( $givenArray ) {
&#xA0; &#xA0; $this-&gt;myArray = $givenArray;
&#xA0; }
&#xA0; function rewind() {
&#xA0; &#xA0; return reset($this-&gt;myArray);
&#xA0; }
&#xA0; function current() {
&#xA0; &#xA0; return current($this-&gt;myArray);
&#xA0; }
&#xA0; function key() {
&#xA0; &#xA0; return key($this-&gt;myArray);
&#xA0; }
&#xA0; function next() {
&#xA0; &#xA0; return next($this-&gt;myArray);
&#xA0; }
&#xA0; function valid() {
&#xA0; &#xA0; return key($this-&gt;myArray) !== null;
&#xA0; }
}

?>
```



  

#



If you have a custom iterator that may throw an exception in it&apos;s current() method, there is no way to catch the exception without breaking a foreach loop.

The following for loop allows you to skip elements for which $iterator-&gt;current() throws an exception, rather than breaking the loop.



```
<?php
for ($iterator-&gt;rewind(); $iterator-&gt;valid(); $iterator-&gt;next()) {
&#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; $value = $iterator-&gt;current();
&#xA0; &#xA0; } catch (Exception $exception) {
&#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; }

&#xA0; &#xA0; # ...
}
?>
```



  

#



It&apos;s important to note that following won&apos;t work if you have null values.



```
<?php
&#xA0; &#xA0; function valid() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__METHOD__);
&#xA0; &#xA0; &#xA0; &#xA0; return isset($this-&gt;array[$this-&gt;position]);
&#xA0; &#xA0; }
?>
```


Other examples have shown the following which won&apos;t work if you have false values:



```
<?php
&#xA0; &#xA0; function valid() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;current() !== false;
&#xA0; &#xA0; }
?>
```


Instead use:



```
<?php
&#xA0; &#xA0; function valid() {
&#xA0; &#xA0; &#xA0; &#xA0; return array_key_exists($this-&gt;array, $this-&gt;position);
&#xA0; &#xA0; }
?>
```


Or the following if you do not store the position.



```
<?php
&#xA0; &#xA0; public function valid() {
&#xA0; &#xA0; &#xA0; &#xA0; return !is_null(key($this-&gt;array));
&#xA0; &#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.iterator.php)

**[To root](/README.md)**