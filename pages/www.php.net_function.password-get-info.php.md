# password_get_info



If you&apos;re curious to use this method to determine if there is someway to evaluate if a given string is NOT a password_hash() value...<br><br>

```
<?php

// Our password.. the kind of thing and idiot would have on his luggage:
$password_plaintext = "12345";

// Hash it up, fuzzball!
$password_hash = password_hash( $password_plaintext, PASSWORD_DEFAULT, [ 'cost' => 11 ] );

// What do we get?
print_r( password_get_info( $password_hash ) );

/* returns:
Array ( 
    [algo] => 1 
    [algoName] => bcrypt  // Your server's default.
    [options] => Array ( [cost] => 11 ) 
)
*/

// What about if it's un-hashed?...
print_r( password_get_info( $password_plaintext ) );

/* returns:
Array ( 
    [algo] => 0 
    [algoName] => unknown 
    [options] => Array ( ) 
) 
*/
?>
```
<br><br>... Looks like it&apos;s up to each of us to personally decide if it&apos;s safe to compare against the final returned array.  

#

[Official documentation page](https://www.php.net/manual/en/function.password-get-info.php)

**[To root](/README.md)**