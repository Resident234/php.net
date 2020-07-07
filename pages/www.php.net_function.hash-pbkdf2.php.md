# hash_pbkdf2





Please pay great attention to the **$length** parameter! It is exactly the **return string length**, NOT the length of raw binary hash result.

I had a big problem about this -- 
I thought that `hash_pbkdf2(...false)` should equals to `bin2hex(hash_pbkdf2(...true))` just like `md5($x)` equals `bin2hex(md5($x, true))`. However I was wrong:

hash_pbkdf2(&apos;sha256&apos;, &apos;123456&apos;, &apos;abc&apos;, 10000, 50, false); // returns string(50) &quot;584bc5b41005169f1fa15177edb78d75f9846afc466a4bae05&quot;
hash_pbkdf2(&apos;sha256&apos;, &apos;123456&apos;, &apos;abc&apos;, 10000, 50, true); // returns string(50) &quot;XK&#x174;&#xFFFD;&#xFFFD;Qw&#xFFFD;u&#xFFFD;&#xFFFD;j&#xFFFD;FjK&#xFFFD;&#xFFFD;&#xFFFD;BFW&#xFFFD;YpG&#xA0; &#xA0; &#xFFFD;mp.g2&#xFFFD;`;N&#xFFFD;&quot;
bin2hex(hash_pbkdf2(&apos;sha256&apos;, &apos;123456&apos;, &apos;abc&apos;, 10000, 50, true)); // returns string(100) &quot;584bc5b41005169f1fa15177edb78d75f9846afc466a4bae05119c82424657c81b5970471f098a6d702e6732b7603b194efe&quot;

So I add such a note. Hope it will help someone else like me.

  

#



This is a light-weight drop-in replacement for PHP&apos;s hash_pbkdf2(); written for compatibility with older versions of PHP.
Written, formatted and tested by myself, but using code and ideas based on the following:
https://defuse.ca/php-pbkdf2.htm
https://github.com/rchouinard/hash_pbkdf2-compat/blob/master/src/hash_pbkdf2.php
https://gist.github.com/rsky/5104756

My main goals:
1) Maximum compatibility with PHP hash_pbkdf2(), ie. a drop-in replacement function
2) Minimum code size/bloat
3) Easy to copy/paste
4) No classes, and not encapsulated in a class! Why write a class when a simple function will do?
5) Eliminate calls to sprintf(). (used by other examples for the error reporting)
6) No other dependencies, ie. extra required functions



```
<?php
if (!function_exists(&apos;hash_pbkdf2&apos;))
{
&#xA0; &#xA0; function hash_pbkdf2($algo, $password, $salt, $count, $length = 0, $raw_output = false)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!in_array(strtolower($algo), hash_algos())) trigger_error(__FUNCTION__ . &apos;(): Unknown hashing algorithm: &apos; . $algo, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; if (!is_numeric($count)) trigger_error(__FUNCTION__ . &apos;(): expects parameter 4 to be long, &apos; . gettype($count) . &apos; given&apos;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; if (!is_numeric($length)) trigger_error(__FUNCTION__ . &apos;(): expects parameter 5 to be long, &apos; . gettype($length) . &apos; given&apos;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; if ($count &lt;= 0) trigger_error(__FUNCTION__ . &apos;(): Iterations must be a positive integer: &apos; . $count, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; if ($length &lt; 0) trigger_error(__FUNCTION__ . &apos;(): Length must be greater than or equal to 0: &apos; . $length, E_USER_WARNING);

&#xA0; &#xA0; &#xA0; &#xA0; $output = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $block_count = $length ? ceil($length / strlen(hash($algo, &apos;&apos;, $raw_output))) : 1;
&#xA0; &#xA0; &#xA0; &#xA0; for ($i = 1; $i &lt;= $block_count; $i++)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $last = $xorsum = hash_hmac($algo, $salt . pack(&apos;N&apos;, $i), $password, true);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for ($j = 1; $j &lt; $count; $j++)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $xorsum ^= ($last = hash_hmac($algo, $last, $password, true));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $output .= $xorsum;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; if (!$raw_output) $output = bin2hex($output);
&#xA0; &#xA0; &#xA0; &#xA0; return $length ? substr($output, 0, $length) : $output;
&#xA0; &#xA0; }
}


  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-pbkdf2.php)

**[To root](/README.md)**