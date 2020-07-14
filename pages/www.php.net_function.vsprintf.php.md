# vsprintf



Instead of inventing own functions in case you&apos;d like to use array keys as placeholder names and replace corresponding array values in a string, just use the str_replace:<br><br>$string = &apos;Hello %name!&apos;;<br>$data = array(<br>  &apos;%name&apos; =&gt; &apos;John&apos;<br>);<br><br>$greeting = str_replace(array_keys($data), array_values($data), $string);  

#



```
<?php
/**
 * Like vsprintf, but accepts $args keys instead of order index.
 * Both numeric and strings matching /[a-zA-Z0-9_-]+/ are allowed.
 *
 * Example: vskprintf(&apos;y = %y$d, x = %x$1.1f&apos;, array(&apos;x&apos; =&gt; 1, &apos;y&apos; =&gt; 2))
 * Result:  &apos;y = 2, x = 1.0&apos;
 *
 * $args also can be object, then it&apos;s properties are retrieved
 * using get_object_vars().
 *
 * &apos;%s&apos; without argument name works fine too. Everything vsprintf() can do
 * is supported.
 *
 * @author Josef Kufner &lt;jkufner(at)gmail.com&gt;
 */
function vksprintf($str, $args)
{
    if (is_object($args)) {
        $args = get_object_vars($args);
    }
    $map = array_flip(array_keys($args));
    $new_str = preg_replace_callback(&apos;/(^|[^%])%([a-zA-Z0-9_-]+)\$/&apos;,
            function($m) use ($map) { return $m[1].&apos;%&apos;.($map[$m[2]] + 1).&apos;

```
<?php<br>/**<br> * Like vsprintf, but accepts $args keys instead of order index.<br> * Both numeric and strings matching /[a-zA-Z0-9_-]+/ are allowed.<br> *<br> * Example: vskprintf(&apos;y = %y$d, x = %x$1.1f&apos;, array(&apos;x&apos; =&gt; 1, &apos;y&apos; =&gt; 2))<br> * Result:  &apos;y = 2, x = 1.0&apos;<br> *<br> * $args also can be object, then it&apos;s properties are retrieved<br> * using get_object_vars().<br> *<br> * &apos;%s&apos; without argument name works fine too. Everything vsprintf() can do<br> * is supported.<br> *<br> * @author Josef Kufner &lt;jkufner(at)gmail.com&gt;<br> */<br>function vksprintf($str, $args)<br>{<br>    if (is_object($args)) {<br>        $args = get_object_vars($args);<br>    }<br>    $map = array_flip(array_keys($args));<br>    $new_str = preg_replace_callback(&apos;/(^|[^%])%([a-zA-Z0-9_-]+)\$/&apos;,<br>            function($m) use ($map) { return $m[1].&apos;%&apos;.($map[$m[2]] + 1).&apos;$&apos;; },<br>            $str);<br>    return vsprintf($new_str, $args);<br>}<br>?>
```
apos;; },
            $str);
    return vsprintf($new_str, $args);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.vsprintf.php)

**[To root](/README.md)**