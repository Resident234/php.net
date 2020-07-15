# password_verify



If you get incorrect false responses from password_verify when manually including the hash variable (eg. for testing) and you know it should be correct, make sure you are enclosing the hash variable in single quotes (&apos;) and not double quotes (").<br><br>PHP parses anything that starts with a $ inside double quotes as a variable:<br><br>

```
<?php
// this will result in 'Invalid Password' as the hash is parsed into 3 variables of
// $2y, $07 and $BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq
// due to it being enclosed inside double quotes
$hash = "$2y$07$BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq";

// this will result in 'Password is valid' as variables are not parsed inside single quotes
$hash = '$2y$07$BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq';

if (password_verify('rasmuslerdorf', $hash)) {
    echo 'Password is valid!';
} else {
    echo 'Invalid password.';
}
?>
```
  

#

This function can be used to verify hashes created with other functions like crypt(). For example:<br><br>

```
<?php

$hash = '$1$toHVx1uW$KIvW9yGZZSU/1YOidHeqJ/';

if (password_verify('rasmuslerdorf', $hash)) {
    echo 'Password is valid!';
} else {
    echo 'Invalid password.';
}

// Output: Password is valid!

?>
```
  

#

The function password_verify() uses constant time. This makes it safe against timing attacks. Don&apos;t use crypt($password_database) === crypt($password_given_by_login), since there is no protection against timing attacks!<br><br>If you don&apos;t want to use password_verify(), then have a look at hash_equals(), which also runs a timing attack safe string comparison.  

#

This Is The Most Secure Way To Keep Your Password Safe With PHP 7 , <br>Even When Your DataBase Has Been Hacked ,<br>It Will Be Almost Impossible To Retrieve Your Password .<br>--------------------------------------------------------<br>--- When A User Wants To Sign Up ---<br>1 ---&gt; Get Input From User Which Is The User`s Password<br>1 ---&gt; Hash The Password<br>2 ---&gt; Store The Hashed Password In Your DataBase<br>--------------------------------------------------------<br>

```
<?php
$hashed_password = password_hash($_POST["password"],PASSWORD_DEFAULT);

// $_POST["password"] ---> Is The User`s Input
// $hashed_password ---> Is The Hashed Password You Can Store In Your DataBase
?>
```

--------------------------------------------------------
--- When A User Wants To Sign In ---
1 ---> Get Input From User Which Is The User`s Password
2 ---> Fetch The Hashed Password From Your Database
3 ---> Compare The User`s Input And The Hashed Password 
--------------------------------------------------------


```
<?php
    if(password_verify($_POST["password"],$hashed_password))
    echo "Welcome"; 

    else
    echo "Wrong Password";

// $_POST["password"] ---> Is The User`s Input
// $hashed_password ---> Is The Hashed Password You Have Fetched From DataBase
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.password-verify.php)

**[To root](/README.md)**