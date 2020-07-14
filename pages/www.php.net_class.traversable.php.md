# The Traversable interface



While you cannot implement this interface, you can use it in your checks to determine if something is usable in for each. Here is what I use if I&apos;m expecting something that must be iterable via foreach.<br><br>

```
<?php
    if( !is_array( $items ) &amp;&amp; !$items instanceof Traversable )
        //Throw exception here
?>
```
  

#

NOTE:  While objects and arrays can be traversed by foreach, they do NOT implement "Traversable", so you CANNOT check for foreach compatibility using an instanceof check.<br><br>Example:<br><br>$myarray = array(&apos;one&apos;, &apos;two&apos;, &apos;three&apos;);<br>$myobj = (object)$myarray;<br><br>if ( !($myarray instanceof \Traversable) ) {<br>    print "myarray is NOT Traversable";<br>}<br>if ( !($myobj instanceof \Traversable) ) {<br>    print "myobj is NOT Traversable";<br>}<br><br>foreach ($myarray as $value) {<br>    print $value;<br>}<br>foreach ($myobj as $value) {<br>    print $value;<br>}<br><br>Output:<br>myarray is NOT Traversable<br>myobj is NOT Traversable<br>one<br>two<br>three<br>one<br>two<br>three  

#

The PHP7 iterable pseudo type will match both Traversable and array. Great for return type-hinting so that you do not have to expose your Domain to Infrastructure code, e.g. instead of a Repository returning a Cursor, it can return hint &apos;iterable&apos;:<br>

```
<?php
UserRepository::findUsers(): iterable
?>
```


Link: http://php.net/manual/en/migration71.new-features.php#migration71.new-features.iterable-pseudo-type

Also, instead of:


```
<?php
    if( !is_array( $items ) &amp;&amp; !$items instanceof Traversable )
        //Throw exception here
?>
```


You can now do with the is_iterable() method:


```
<?php
    if ( !is_iterable( $items ))
        //Throw exception here
?>
```
<br><br>Link:  http://php.net/manual/en/function.is-iterable.php  

#

Note that all objects can be iterated over with foreach anyway and it&apos;ll go over each property. This just describes whether or not the class implements an iterator, i.e. has custom behaviour.  

#

[Official documentation page](https://www.php.net/manual/en/class.traversable.php)

**[To root](/README.md)**