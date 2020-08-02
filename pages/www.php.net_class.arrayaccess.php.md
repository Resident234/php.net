# The ArrayAccess interface



It bit me today, so putting it here in the hope it will help others:<br>If you call array_key_exists() on an object of a class that implements ArrayAccess, ArrayAccess::offsetExists() wil NOT be called.  

---

The indexes used in an ArrayAccess object are not limited to strings and integers as they are for arrays: you can use any type for the index as long as you write your implementation to handle them. This fact is exploited by the SplObjectStorage class.  

---



```
<?php

/**
 * ArrayAndObjectAccess
 * Yes you can access class as array and the same time as object
 *
 * @author Yousef Ismaeil <cliprz@gmail.com>
 */

class ArrayAndObjectAccess implements ArrayAccess {

    /**
     * Data
     *
     * @var array
     * @access private
     */
    private $data = [];

    /**
     * Get a data by key
     *
     * @param string The key data to retrieve
     * @access public
     */
    public function &amp;__get ($key) {
        return $this->data[$key];
    }

    /**
     * Assigns a value to the specified data
     * 
     * @param string The data key to assign the value to
     * @param mixed  The value to set
     * @access public 
     */
    public function __set($key,$value) {
        $this->data[$key] = $value;
    }

    /**
     * Whether or not an data exists by key
     *
     * @param string An data key to check for
     * @access public
     * @return boolean
     * @abstracting ArrayAccess
     */
    public function __isset ($key) {
        return isset($this->data[$key]);
    }

    /**
     * Unsets an data by key
     *
     * @param string The key to unset
     * @access public
     */
    public function __unset($key) {
        unset($this->data[$key]);
    }

    /**
     * Assigns a value to the specified offset
     *
     * @param string The offset to assign the value to
     * @param mixed  The value to set
     * @access public
     * @abstracting ArrayAccess
     */
    public function offsetSet($offset,$value) {
        if (is_null($offset)) {
            $this->data[] = $value;
        } else {
            $this->data[$offset] = $value;
        }
    }

    /**
     * Whether or not an offset exists
     *
     * @param string An offset to check for
     * @access public
     * @return boolean
     * @abstracting ArrayAccess
     */
    public function offsetExists($offset) {
        return isset($this->data[$offset]);
    }

    /**
     * Unsets an offset
     *
     * @param string The offset to unset
     * @access public
     * @abstracting ArrayAccess
     */
    public function offsetUnset($offset) {
        if ($this->offsetExists($offset)) {
            unset($this->data[$offset]);
        }
    }

    /**
     * Returns the value at specified offset
     *
     * @param string The offset to retrieve
     * @access public
     * @return mixed
     * @abstracting ArrayAccess
     */
    public function offsetGet($offset) {
        return $this->offsetExists($offset) ? $this->data[$offset] : null;
    }

}

?>
```


Usage



```
<?php
$foo = new ArrayAndObjectAccess();
// Set data as array and object
$foo->fname = 'Yousef';
$foo->lname = 'Ismaeil';
// Call as object
echo 'fname as object '.$foo->fname."\n";
// Call as array
echo 'lname as array '.$foo['lname']."\n";
// Reset as array
$foo['fname'] = 'Cliprz';
echo $foo['fname']."\n";

/** Outputs
fname as object Yousef
lname as array Ismaeil
Cliprz
*/

?>
```
  

---

Objects implementing ArrayAccess may return objects by references in PHP 5.3.0.<br><br>You can implement your ArrayAccess object like this:<br><br>    class Reflectable implements ArrayAccess {<br><br>        public function set($name, $value) {<br>            $this-&gt;{$name} = $value;<br>        }<br><br>        public function &amp;get($name) {<br>            return $this-&gt;{$name};<br>        }<br><br>        public function offsetGet($offset) {<br>            return $this-&gt;get($offset);<br>        }<br><br>        public function offsetSet($offset, $value) {<br>            $this-&gt;set($offset, $value);<br>        }<br><br>        ...<br><br>    }<br><br>This base class allows you to get / set your object properties using the [] operator just like in Javascript:<br><br>    class Boo extends Reflectable {<br>        public $name;<br>    }<br><br>    $obj = new Boo();<br>    $obj[&apos;name&apos;] = "boo";<br>    echo $obj[&apos;name&apos;]; // prints boo  

---

Conclusion: Type hints \ArrayAccess and array are not compatible.<br><br>

```
<?php

     class MyArrayAccess implements \ArrayAccess
     {
         public function offsetExists($offset)
         {

         }

         public function offsetSet($offset, $value)
         {

         }

         public function offsetGet($offset)
         {

         }

         public function offsetUnset($offset)
         {

         }
     }


     function test(array $arr)
     {
     }

     function test2(\ArrayAccess $arr)
     {

     }


     $arrObj = new MyArrayAccess();
     test([]); //result: works!
     test($arrObj); //result: does NOT work
     test2([]); //result: does NOT work
     test2($arrObj); // result: works!
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.arrayaccess.php)

**[To root](/README.md)**