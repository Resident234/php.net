# is_iterable




<div class="phpcode"><span class="html">
A slight correction to brcontainer&apos;s polyfill, which prevents errors on a non-object in a non-blocking way, and also corrects the issue of&#xA0; the conditional checking &quot;file_exists&quot; instead of the correct &quot;function_exists&quot;:<br><br>if ( !function_exists(&#xA0; &apos;is_iterable&apos; ) )<br>{<br><br>&#xA0; &#xA0; function is_iterable( $obj )<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return is_array( $obj ) || ( is_object( $obj ) &amp;&amp; ( $obj instanceof \Traversable ) );<br>&#xA0; &#xA0; }<br><br>}<br><br>The original answer would not have resolved correctly, because it was looking for a file instead of a function, and the provided method would error if given a non-iterable non-object value such as false.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-iterable.php)

**[⬆ to root](/)**