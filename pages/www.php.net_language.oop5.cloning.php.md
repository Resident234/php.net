# Object Cloning



I think it&apos;s relevant to note that __clone is NOT an override. As the example shows, the normal cloning process always occurs, and it&apos;s the responsibility of the __clone method to "mend" any "wrong" action performed by it.  

#

Here is test script i wrote to test the behaviour of clone when i have arrays with primitive values in my class - as an additonal test of the note below by jeffrey at whinger dot nl<br><br>&lt;pre&gt;<br>

```
<?php

class MyClass {

    private $myArray = array();
    function pushSomethingToArray($var) {
        array_push($this->myArray, $var);
    }
    function getArray() {
        return $this->myArray;
    }

}

//push some values to the myArray of Mainclass
$myObj = new MyClass();
$myObj->pushSomethingToArray('blue');
$myObj->pushSomethingToArray('orange');
$myObjClone = clone $myObj;
$myObj->pushSomethingToArray('pink');

//testing
print_r($myObj->getArray());     //Array([0] => blue,[1] => orange,[2] => pink)
print_r($myObjClone->getArray());//Array([0] => blue,[1] => orange)
//so array  cloned 

?>
```
<br>&lt;/pre&gt;  

#

I ran into the same problem of an array of objects inside of an object that I wanted to clone all pointing to the same objects. However, I agreed that serializing the data was not the answer. It was relatively simple, really:<br><br>public function __clone() {<br>    foreach ($this-&gt;varName as &amp;$a) {<br>        foreach ($a as &amp;$b) {<br>            $b = clone $b;<br>        }<br>    }<br>}<br><br>Note, that I was working with a multi-dimensional array and I was not using the Key=&gt;Value pair system, but basically, the point is that if you use foreach, you need to specify that the copied data is to be accessed by reference.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.cloning.php)

**[To root](/README.md)**