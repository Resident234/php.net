# password_needs_rehash



nick, this function cannot check if a string is a MD5 or SHA1 hash. It can only tell you if a password, hashed using the password_hash function, needs to be put through the hashing function again to keep up to date with the new defaults.<br><br>The only time you can use this function is when your user logs in and you have already checked by means of password_verify that the password entered is actually correct. At that point, if password_needs_rehash returns true, you can put the plain text password through the password_hash function.  

---

ydroneaud this would be used on a login page, not at any other time.<br><br>So if you have a site with MD5 passwords for example, and wish to upgrade to SHA256 for additional security you would put this check in the login script.<br><br>This function will take a user&apos;s hash and say if it is SHA256, if it isn&apos;t then you can take the user&apos;s password which you still have as plaintext and rehash it as SHA256.<br><br>This lets you gradually update the hashes in your database without disrupting any features or resetting passwords.  

---

Some other use-cases for the password_needs_rehash function is when you have specified using the PASSWORD_DEFAULT algorithm for password_hash.<br>As mentioned on the Password Hashing Predefined Constants and password_hash pages, the algorithm used by PASSWORD_DEFAULT is subject to change as different versions of PHP are released.<br>Additionally password_needs_rehash would be used if you have changed the optional cost or static salt (DO NOT USE A STATIC SALT) requirements of your password_hash options.<br><br>Full example:<br><br>

```
<?php

$new = [
    'options' => ['cost' => 11],
    'algo' => PASSWORD_DEFAULT,
    'hash' => null
];

$password = 'rasmuslerdorf';

//stored hash of password
$oldHash = '$2y$07$BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq';

//verify stored hash against plain-text password
if (true === password_verify($password, $oldHash)) {
    //verify legacy password to new password_hash options
    if (true === password_needs_rehash($oldHash, $new['algo'], $new['options'])) {
        //rehash/store plain-text password using new hash
        $newHash = password_hash($password, $new['algo'], $new['options']);
        echo $newHash;
    }
}
?>
```
<br><br>The above example will output something similar to:<br>$2y$11$Wu5rN3u38.g/XWdUeA6Wj.PD.F0fLXXmZrMNFyzzg2UxkVmxlk41W  

---

[Official documentation page](https://www.php.net/manual/en/function.password-needs-rehash.php)

**[To root](/README.md)**