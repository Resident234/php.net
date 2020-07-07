# password_get_info





If you&apos;re curious to use this method to determine if there is someway to evaluate if a given string is NOT a password_hash() value...



```
<?php

// Our password.. the kind of thing and idiot would have on his luggage:
$password_plaintext = &quot;12345&quot;;

// Hash it up, fuzzball!
$password_hash = password_hash( $password_plaintext, PASSWORD_DEFAULT, [ &apos;cost&apos; =&gt; 11 ] );

// What do we get?
print_r( password_get_info( $password_hash ) );

/* returns:
Array ( 
&#xA0; &#xA0; [algo] =&gt; 1 
&#xA0; &#xA0; [algoName] =&gt; bcrypt&#xA0; // Your server&apos;s default.
&#xA0; &#xA0; [options] =&gt; Array ( [cost] =&gt; 11 ) 
)
*/

// What about if it&apos;s un-hashed?...
print_r( password_get_info( $password_plaintext ) );

/* returns:
Array ( 
&#xA0; &#xA0; [algo] =&gt; 0 
&#xA0; &#xA0; [algoName] =&gt; unknown 
&#xA0; &#xA0; [options] =&gt; Array ( ) 
) 
*/
?>
```


... Looks like it&apos;s up to each of us to personally decide if it&apos;s safe to compare against the final returned array.

  

#

[Official documentation page](https://www.php.net/manual/en/function.password-get-info.php)

**[To root](/README.md)**