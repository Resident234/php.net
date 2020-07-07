# HTTP authentication with PHP





Workaround for missing Authorization header under CGI/FastCGI Apache:

SetEnvIf Authorization .+ HTTP_AUTHORIZATION=$0

Now PHP should automatically declare $_SERVER[PHP_AUTH_*] variables if the client sends the Authorization header.

  

#



This is the simplest form I found to do a Basic authorization with retries.



```
<?php

$valid_passwords = array (&quot;mario&quot; =&gt; &quot;carbonell&quot;);
$valid_users = array_keys($valid_passwords);

$user = $_SERVER[&apos;PHP_AUTH_USER&apos;];
$pass = $_SERVER[&apos;PHP_AUTH_PW&apos;];

$validated = (in_array($user, $valid_users)) &amp;&amp; ($pass == $valid_passwords[$user]);

if (!$validated) {
&#xA0; header(&apos;WWW-Authenticate: Basic realm=&quot;My Realm&quot;&apos;);
&#xA0; header(&apos;HTTP/1.0 401 Unauthorized&apos;);
&#xA0; die (&quot;Not authorized&quot;);
}

// If arrives here, is a valid user.
echo &quot;&lt;p&gt;Welcome $user.&lt;/p&gt;&quot;;
echo &quot;&lt;p&gt;Congratulation, you are into the system.&lt;/p&gt;&quot;;

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.http-auth.php)

**[To root](/README.md)**