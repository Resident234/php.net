# is_iterable



A slight correction to brcontainer&apos;s polyfill, which prevents errors on a non-object in a non-blocking way, and also corrects the issue of  the conditional checking "file_exists" instead of the correct "function_exists":<br><br>if ( !function_exists(  &apos;is_iterable&apos; ) )<br>{<br><br>    function is_iterable( $obj )<br>    {<br>        return is_array( $obj ) || ( is_object( $obj ) &amp;&amp; ( $obj instanceof \Traversable ) );<br>    }<br><br>}<br><br>The original answer would not have resolved correctly, because it was looking for a file instead of a function, and the provided method would error if given a non-iterable non-object value such as false.  

---

[Official documentation page](https://www.php.net/manual/en/function.is-iterable.php)

**[To root](/README.md)**