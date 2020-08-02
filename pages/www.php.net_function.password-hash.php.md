# password_hash



There is a compatibility pack available for PHP versions 5.3.7 and later, so you don&apos;t have to wait on version 5.5 for using this function. It comes in form of a single php file:<br>https://github.com/ircmaxell/password_compat  

---

Since 2017, NIST recommends using a secret input when hashing memorized secrets such as passwords. By mixing in a secret input (commonly called a "pepper"), one prevents an attacker from brute-forcing the password hashes altogether, even if they have the hash and salt. For example, an SQL injection typically affects only the database, not files on disk, so a pepper stored in a config file would still be out of reach for the attacker. A pepper must be randomly generated once and can be the same for all users. Many password leaks could have been made completely useless if site owners had done this.<br><br>Since there is no pepper parameter for password_hash (even though Argon2 has a "secret" parameter, PHP does not allow to set it), the correct way to mix in a pepper is to use hash_hmac(). The "add note" rules of php.net say I can&apos;t link external sites, so I can&apos;t back any of this up with a link to NIST, Wikipedia, posts from the security stackexchange site that explain the reasoning, or anything... You&apos;ll have to verify this manually. The code:<br><br>// config.conf<br>pepper=c1isvFdxMDdmjOlvxpecFw<br><br>

```
<?php
// register.php
$pepper = getConfigVariable("pepper");
$pwd = $_POST['password'];
$pwd_peppered = hash_hmac("sha256", $pwd, $pepper);
$pwd_hashed = password_hash($pwd_peppered, PASSWORD_ARGON2ID);
add_user_to_database($username, $pwd_hashed);
?>
```




```
<?php
// login.php
$pepper = getConfigVariable("pepper");
$pwd = $_POST['password'];
$pwd_peppered = hash_hmac("sha256", $pwd, $pepper);
$pwd_hashed = get_pwd_from_db($username);
if (password_verify($pwd_peppered, $pwd_hashed)) {
    echo "Password matches.";
}
else {
    echo "Password incorrect.";
}
?>
```
<br><br>Note that this code contains a timing attack that leaks whether the username exists. But my note was over the length limit so I had to cut this paragraph out.<br><br>Also note that the pepper is useless if leaked or if it can be cracked. Consider how it might be exposed, for example different methods of passing it to a docker container. Against cracking, use a long randomly generated value (like in the example above), and change the pepper when you do a new install with a clean user database. Changing the pepper for an existing database is the same as changing other hashing parameters: you can either wrap the old value in a new one and layer the hashing (more complex), you compute the new password hash whenever someone logs in (leaving old users at risk, so this might be okay depending on what the reason is that you&apos;re upgrading).<br><br>Why does this work? Because an attacker does the following after stealing the database:<br><br>password_verify("a", $stolen_hash)<br>password_verify("b", $stolen_hash)<br>...<br>password_verify("z", $stolen_hash)<br>password_verify("aa", $stolen_hash)<br>etc.<br><br>(More realistically, they use a cracking dictionary, but in principle, the way to crack a password hash is by guessing. That&apos;s why we use special algorithms: they are slower, so each verify() operation will be slower, so they can try much fewer passwords per hour of cracking.)<br><br>Now what if you used that pepper? Now they need to do this:<br><br>password_verify(hmac_sha256("a", $secret), $stolen_hash)<br><br>Without that $secret (the pepper), they can&apos;t do this computation. They would have to do:<br><br>password_verify(hmac_sha256("a", "a"), $stolen_hash)<br>password_verify(hmac_sha256("a", "b"), $stolen_hash)<br>...<br>etc., until they found the correct pepper.<br><br>If your pepper contains 128 bits of entropy, and so long as hmac-sha256 remains secure (even MD5 is technically secure for use in hmac: only its collision resistance is broken, but of course nobody would use MD5 because more and more flaws are found), this would take more energy than the sun outputs. In other words, it&apos;s currently impossible to crack a pepper that strong, even given a known password and salt.  

---

I agree with martinstoeckli,<br><br>don&apos;t create your own salts unless you really know what you&apos;re doing.<br><br>By default, it&apos;ll use /dev/urandom to create the salt, which is based on noise from device drivers.<br><br>And on Windows, it uses CryptGenRandom().<br><br>Both have been around for many years, and are considered secure for cryptography (the former probably more than the latter, though).<br><br>Don&apos;t try to outsmart these defaults by creating something less secure. Anything that is based on rand(), mt_rand(), uniqid(), or variations of these is *not* good.  

---

You can produce the same hash in php 5.3.7+ with crypt() function:<br><br>

```
<?php

$salt = mcrypt_create_iv(22, MCRYPT_DEV_URANDOM);
$salt = base64_encode($salt);
$salt = str_replace('+', '.', $salt);
$hash = crypt('rasmuslerdorf', '$2y$10  .$salt.'  );

echo $hash;

?>
```
  

---

Please note that password_hash will ***truncate*** the password at the first NULL-byte.<br><br>http://blog.ircmaxell.com/2015/03/security-issue-combining-bcrypt-with.html<br><br>If you use anything as an input that can generate NULL bytes (sha1 with raw as true, or if NULL bytes can naturally end up in people&apos;s passwords), you may make your application much less secure than what you might be expecting.<br><br>The password <br>$a = "\01234567"; <br>is zero bytes long (an empty password) for bcrypt.<br><br>The workaround, of course, is to make sure you don&apos;t ever pass NULL-bytes to password_hash.  

---

In most cases it is best to omit the salt parameter. Without this parameter, the function will generate a cryptographically safe salt, from the random source of the operating system.  

---

[Official documentation page](https://www.php.net/manual/en/function.password-hash.php)

**[To root](/README.md)**