# The ArrayAccess interface





It bit me today, so putting it here in the hope it will help others:
If you call array_key_exists() on an object of a class that implements ArrayAccess, ArrayAccess::offsetExists() wil NOT be called.

  

#



The indexes used in an ArrayAccess object are not limited to strings and integers as they are for arrays: you can use any type for the index as long as you write your implementation to handle them. This fact is exploited by the SplObjectStorage class.

  

#





```
<?php

/**
 * ArrayAndObjectAccess
 * Yes you can access class as array and the same time as object
 *
 * @author Yousef Ismaeil &lt;cliprz@gmail.com&gt;
 */

class ArrayAndObjectAccess implements ArrayAccess {

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Data
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @var array
&#xA0; &#xA0;&#xA0; * @access private
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; private $data = [];

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Get a data by key
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string The key data to retrieve
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function &amp;__get ($key) {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;data[$key];
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Assigns a value to the specified data
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * @param string The data key to assign the value to
&#xA0; &#xA0;&#xA0; * @param mixed&#xA0; The value to set
&#xA0; &#xA0;&#xA0; * @access public 
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __set($key,$value) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;data[$key] = $value;
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Whether or not an data exists by key
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string An data key to check for
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; * @return boolean
&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __isset ($key) {
&#xA0; &#xA0; &#xA0; &#xA0; return isset($this-&gt;data[$key]);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Unsets an data by key
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string The key to unset
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __unset($key) {
&#xA0; &#xA0; &#xA0; &#xA0; unset($this-&gt;data[$key]);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Assigns a value to the specified offset
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string The offset to assign the value to
&#xA0; &#xA0;&#xA0; * @param mixed&#xA0; The value to set
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function offsetSet($offset,$value) {
&#xA0; &#xA0; &#xA0; &#xA0; if (is_null($offset)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;data[] = $value;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;data[$offset] = $value;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Whether or not an offset exists
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string An offset to check for
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; * @return boolean
&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function offsetExists($offset) {
&#xA0; &#xA0; &#xA0; &#xA0; return isset($this-&gt;data[$offset]);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Unsets an offset
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string The offset to unset
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function offsetUnset($offset) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($this-&gt;offsetExists($offset)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($this-&gt;data[$offset]);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Returns the value at specified offset
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string The offset to retrieve
&#xA0; &#xA0;&#xA0; * @access public
&#xA0; &#xA0;&#xA0; * @return mixed
&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function offsetGet($offset) {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;offsetExists($offset) ? $this-&gt;data[$offset] : null;
&#xA0; &#xA0; }

}

?>
```


Usage



```
<?php
$foo = new ArrayAndObjectAccess();
// Set data as array and object
$foo-&gt;fname = &apos;Yousef&apos;;
$foo-&gt;lname = &apos;Ismaeil&apos;;
// Call as object
echo &apos;fname as object &apos;.$foo-&gt;fname.&quot;\n&quot;;
// Call as array
echo &apos;lname as array &apos;.$foo[&apos;lname&apos;].&quot;\n&quot;;
// Reset as array
$foo[&apos;fname&apos;] = &apos;Cliprz&apos;;
echo $foo[&apos;fname&apos;].&quot;\n&quot;;

/** Outputs
fname as object Yousef
lname as array Ismaeil
Cliprz
*/

?>
```



  

#



Objects implementing ArrayAccess may return objects by references in PHP 5.3.0.

You can implement your ArrayAccess object like this:

&#xA0; &#xA0; class Reflectable implements ArrayAccess {

&#xA0; &#xA0; &#xA0; &#xA0; public function set($name, $value) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;{$name} = $value;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function &amp;get($name) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;{$name};
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function offsetGet($offset) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;get($offset);
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function offsetSet($offset, $value) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;set($offset, $value);
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; ...

&#xA0; &#xA0; }

This base class allows you to get / set your object properties using the [] operator just like in Javascript:

&#xA0; &#xA0; class Boo extends Reflectable {
&#xA0; &#xA0; &#xA0; &#xA0; public $name;
&#xA0; &#xA0; }

&#xA0; &#xA0; $obj = new Boo();
&#xA0; &#xA0; $obj[&apos;name&apos;] = &quot;boo&quot;;
&#xA0; &#xA0; echo $obj[&apos;name&apos;]; // prints boo

  

#



Conclusion: Type hints \ArrayAccess and array are not compatible.





```
<?php



&#xA0; &#xA0;&#xA0; class MyArrayAccess implements \ArrayAccess

&#xA0; &#xA0;&#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function offsetExists($offset)

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function offsetSet($offset, $value)

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function offsetGet($offset)

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function offsetUnset($offset)

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {



&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; }





&#xA0; &#xA0;&#xA0; function test(array $arr)

&#xA0; &#xA0;&#xA0; {

&#xA0; &#xA0;&#xA0; }



&#xA0; &#xA0;&#xA0; function test2(\ArrayAccess $arr)

&#xA0; &#xA0;&#xA0; {



&#xA0; &#xA0;&#xA0; }





&#xA0; &#xA0;&#xA0; $arrObj = new MyArrayAccess();

&#xA0; &#xA0;&#xA0; test([]); //result: works!

&#xA0; &#xA0;&#xA0; test($arrObj); //result: does NOT work

&#xA0; &#xA0;&#xA0; test2([]); //result: does NOT work

&#xA0; &#xA0;&#xA0; test2($arrObj); // result: works!

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.arrayaccess.php)

**[To root](/README.md)**