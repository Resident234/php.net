# The Traversable interface





While you cannot implement this interface, you can use it in your checks to determine if something is usable in for each. Here is what I use if I&apos;m expecting something that must be iterable via foreach.



```
<?php
&#xA0; &#xA0; if( !is_array( $items ) &amp;&amp; !$items instanceof Traversable )
&#xA0; &#xA0; &#xA0; &#xA0; //Throw exception here
?>
```



  

#



NOTE:&#xA0; While objects and arrays can be traversed by foreach, they do NOT implement &quot;Traversable&quot;, so you CANNOT check for foreach compatibility using an instanceof check.

Example:

$myarray = array(&apos;one&apos;, &apos;two&apos;, &apos;three&apos;);
$myobj = (object)$myarray;

if ( !($myarray instanceof \Traversable) ) {
&#xA0; &#xA0; print &quot;myarray is NOT Traversable&quot;;
}
if ( !($myobj instanceof \Traversable) ) {
&#xA0; &#xA0; print &quot;myobj is NOT Traversable&quot;;
}

foreach ($myarray as $value) {
&#xA0; &#xA0; print $value;
}
foreach ($myobj as $value) {
&#xA0; &#xA0; print $value;
}

Output:
myarray is NOT Traversable
myobj is NOT Traversable
one
two
three
one
two
three

  

#



The PHP7 iterable pseudo type will match both Traversable and array. Great for return type-hinting so that you do not have to expose your Domain to Infrastructure code, e.g. instead of a Repository returning a Cursor, it can return hint &apos;iterable&apos;:


```
<?php
UserRepository::findUsers(): iterable
?>
```


Link: http://php.net/manual/en/migration71.new-features.php#migration71.new-features.iterable-pseudo-type

Also, instead of:


```
<?php
&#xA0; &#xA0; if( !is_array( $items ) &amp;&amp; !$items instanceof Traversable )
&#xA0; &#xA0; &#xA0; &#xA0; //Throw exception here
?>
```


You can now do with the is_iterable() method:


```
<?php
&#xA0; &#xA0; if ( !is_iterable( $items ))
&#xA0; &#xA0; &#xA0; &#xA0; //Throw exception here
?>
```


Link:&#xA0; http://php.net/manual/en/function.is-iterable.php

  

#



Note that all objects can be iterated over with foreach anyway and it&apos;ll go over each property. This just describes whether or not the class implements an iterator, i.e. has custom behaviour.

  

#

[Official documentation page](https://www.php.net/manual/en/class.traversable.php)

**[To root](/README.md)**