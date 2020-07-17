# hash_pbkdf2



Please pay great attention to the **$length** parameter! It is exactly the **return string length**, NOT the length of raw binary hash result.<br><br>I had a big problem about this -- <br>I thought that `hash_pbkdf2(...false)` should equals to `bin2hex(hash_pbkdf2(...true))` just like `md5($x)` equals `bin2hex(md5($x, true))`. However I was wrong:<br><br>hash_pbkdf2(&apos;sha256&apos;, &apos;123456&apos;, &apos;abc&apos;, 10000, 50, false); // returns string(50) "584bc5b41005169f1fa15177edb78d75f9846afc466a4bae05"<br>hash_pbkdf2(&apos;sha256&apos;, &apos;123456&apos;, &apos;abc&apos;, 10000, 50, true); // returns string(50) "XK&#x174;&#xFFFD;&#xFFFD;Qw&#xFFFD;u&#xFFFD;&#xFFFD;j&#xFFFD;FjK&#xFFFD;&#xFFFD;&#xFFFD;BFW&#xFFFD;YpG    &#xFFFD;mp.g2&#xFFFD;`;N&#xFFFD;"<br>bin2hex(hash_pbkdf2(&apos;sha256&apos;, &apos;123456&apos;, &apos;abc&apos;, 10000, 50, true)); // returns string(100) "584bc5b41005169f1fa15177edb78d75f9846afc466a4bae05119c82424657c81b5970471f098a6d702e6732b7603b194efe"<br><br>So I add such a note. Hope it will help someone else like me.  

#

This is a light-weight drop-in replacement for PHP&apos;s hash_pbkdf2(); written for compatibility with older versions of PHP.<br>Written, formatted and tested by myself, but using code and ideas based on the following:<br>https://defuse.ca/php-pbkdf2.htm<br>https://github.com/rchouinard/hash_pbkdf2-compat/blob/master/src/hash_pbkdf2.php<br>https://gist.github.com/rsky/5104756<br><br>My main goals:<br>1) Maximum compatibility with PHP hash_pbkdf2(), ie. a drop-in replacement function<br>2) Minimum code size/bloat<br>3) Easy to copy/paste<br>4) No classes, and not encapsulated in a class! Why write a class when a simple function will do?<br>5) Eliminate calls to sprintf(). (used by other examples for the error reporting)<br>6) No other dependencies, ie. extra required functions<br><br>

```
<?php
if (!function_exists('hash_pbkdf2'))
{
    function hash_pbkdf2($algo, $password, $salt, $count, $length = 0, $raw_output = false)
    {
        if (!in_array(strtolower($algo), hash_algos())) trigger_error(__FUNCTION__ . '(): Unknown hashing algorithm: ' . $algo, E_USER_WARNING);
        if (!is_numeric($count)) trigger_error(__FUNCTION__ . '(): expects parameter 4 to be long, ' . gettype($count) . ' given', E_USER_WARNING);
        if (!is_numeric($length)) trigger_error(__FUNCTION__ . '(): expects parameter 5 to be long, ' . gettype($length) . ' given', E_USER_WARNING);
        if ($count <= 0) trigger_error(__FUNCTION__ . '(): Iterations must be a positive integer: ' . $count, E_USER_WARNING);
        if ($length < 0) trigger_error(__FUNCTION__ . '(): Length must be greater than or equal to 0: ' . $length, E_USER_WARNING);

        $output = '';
        $block_count = $length ? ceil($length / strlen(hash($algo, '', $raw_output))) : 1;
        for ($i = 1; $i <= $block_count; $i++)
        {
            $last = $xorsum = hash_hmac($algo, $salt . pack('N', $i), $password, true);
            for ($j = 1; $j < $count; $j++)
            {
                $xorsum ^= ($last = hash_hmac($algo, $last, $password, true));
            }
            $output .= $xorsum;
        }

        if (!$raw_output) $output = bin2hex($output);
        return $length ? substr($output, 0, $length) : $output;
    }
}?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-pbkdf2.php)

**[To root](/README.md)**