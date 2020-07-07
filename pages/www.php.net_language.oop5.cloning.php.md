# Object Cloning





I think it&apos;s relevant to note that __clone is NOT an override. As the example shows, the normal cloning process always occurs, and it&apos;s the responsibility of the __clone method to &quot;mend&quot; any &quot;wrong&quot; action performed by it.

  

#



Here is test script i wrote to test the behaviour of clone when i have arrays with primitive values in my class - as an additonal test of the note below by jeffrey at whinger dot nl

&lt;pre&gt;


```
<?php

class MyClass {

&#xA0; &#xA0; private $myArray = array();
&#xA0; &#xA0; function pushSomethingToArray($var) {
&#xA0; &#xA0; &#xA0; &#xA0; array_push($this-&gt;myArray, $var);
&#xA0; &#xA0; }
&#xA0; &#xA0; function getArray() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;myArray;
&#xA0; &#xA0; }

}

//push some values to the myArray of Mainclass
$myObj = new MyClass();
$myObj-&gt;pushSomethingToArray(&apos;blue&apos;);
$myObj-&gt;pushSomethingToArray(&apos;orange&apos;);
$myObjClone = clone $myObj;
$myObj-&gt;pushSomethingToArray(&apos;pink&apos;);

//testing
print_r($myObj-&gt;getArray());&#xA0; &#xA0;&#xA0; //Array([0] =&gt; blue,[1] =&gt; orange,[2] =&gt; pink)
print_r($myObjClone-&gt;getArray());//Array([0] =&gt; blue,[1] =&gt; orange)
//so array&#xA0; cloned 

?>
```

&lt;/pre&gt;

  

#



I ran into the same problem of an array of objects inside of an object that I wanted to clone all pointing to the same objects. However, I agreed that serializing the data was not the answer. It was relatively simple, really:



public function __clone() {

&#xA0; &#xA0; foreach ($this-&gt;varName as &amp;$a) {

&#xA0; &#xA0; &#xA0; &#xA0; foreach ($a as &amp;$b) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $b = clone $b;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}



Note, that I was working with a multi-dimensional array and I was not using the Key=&gt;Value pair system, but basically, the point is that if you use foreach, you need to specify that the copied data is to be accessed by reference.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.cloning.php)

**[To root](/README.md)**