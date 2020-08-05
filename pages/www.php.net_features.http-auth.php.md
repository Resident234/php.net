# HTTP authentication with PHP



Workaround for missing Authorization header under CGI/FastCGI Apache:<br><br>SetEnvIf Authorization .+ HTTP_AUTHORIZATION=$0<br><br>Now PHP should automatically declare $_SERVER[PHP_AUTH_*] variables if the client sends the Authorization header.  

---

This is the simplest form I found to do a Basic authorization with retries.<br><br>

```
<?php

$valid_passwords = array ("mario" => "carbonell");
$valid_users = array_keys($valid_passwords);

$user = $_SERVER['PHP_AUTH_USER'];
$pass = $_SERVER['PHP_AUTH_PW'];

$validated = (in_array($user, $valid_users)) &amp;&amp; ($pass == $valid_passwords[$user]);

if (!$validated) {
  header('WWW-Authenticate: Basic realm="My Realm"');
  header('HTTP/1.0 401 Unauthorized');
  die ("Not authorized");
}

// If arrives here, is a valid user.
echo "<p>Welcome $user.</p>";
echo "<p>Congratulation, you are into the system.</p>";

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/features.http-auth.php)

**[To root](/README.md)**