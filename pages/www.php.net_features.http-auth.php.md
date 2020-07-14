# HTTP authentication with PHP



Workaround for missing Authorization header under CGI/FastCGI Apache:<br><br>SetEnvIf Authorization .+ HTTP_AUTHORIZATION=$0<br><br>Now PHP should automatically declare $_SERVER[PHP_AUTH_*] variables if the client sends the Authorization header.  

#

This is the simplest form I found to do a Basic authorization with retries.<br><br>

```
<?php

$valid_passwords = array ("mario" =&gt; "carbonell");
$valid_users = array_keys($valid_passwords);

$user = $_SERVER[&apos;PHP_AUTH_USER&apos;];
$pass = $_SERVER[&apos;PHP_AUTH_PW&apos;];

$validated = (in_array($user, $valid_users)) &amp;&amp; ($pass == $valid_passwords[$user]);

if (!$validated) {
  header(&apos;WWW-Authenticate: Basic realm="My Realm"&apos;);
  header(&apos;HTTP/1.0 401 Unauthorized&apos;);
  die ("Not authorized");
}

// If arrives here, is a valid user.
echo "&lt;p&gt;Welcome $user.&lt;/p&gt;";
echo "&lt;p&gt;Congratulation, you are into the system.&lt;/p&gt;";

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/features.http-auth.php)

**[To root](/README.md)**