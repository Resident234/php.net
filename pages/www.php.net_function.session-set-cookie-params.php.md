# session_set_cookie_params



As PHP&apos;s Session Control does not handle session lifetimes correctly when using session_set_cookie_params(), we need to do something in order to change the session expiry time every time the user visits our site. So, here&apos;s the problem.<br><br>

```
<?php
  $lifetime=600;
  session_set_cookie_params($lifetime);
  session_start();
?>
```


This code doesn't change the lifetime of the session when the user gets back at our site or refreshes the page. The session WILL expire after $lifetime seconds, no matter how many times the user requests the page. So we just overwrite the session cookie as follows:



```
<?php
  $lifetime=600;
  session_start();
  setcookie(session_name(),session_id(),time()+$lifetime);
?>
```
<br><br>And now we have the same session cookie with the lifetime set to the proper value.  

---

Setting the domain for cookies in session_set_cookie_params() only affects the domain used for the session cookie which is set by PHP.<br><br>All other cookies set by calling the function setcookie() either:<br>i) Use the domain set explicitly in the call to setcookie()<br>or <br>ii) Don&apos;t set the domain at all on the cookie and so the browser assumes it&apos;s for the current domain.<br><br>So to make all your cookies be available across all sub-domains of your site you need to do this:<br><br>

```
<?php
$currentCookieParams = session_get_cookie_params();

$rootDomain = '.example.com';

session_set_cookie_params(
    $currentCookieParams["lifetime"],
    $currentCookieParams["path"],
    $rootDomain,
    $currentCookieParams["secure"],
    $currentCookieParams["httponly"]
);

session_name('mysessionname');
session_start();

setcookie($cookieName, $cookieValue, time() + 3600, '/', $rootDomain);
?>
```
  

---

Please take note of the garbage collection "feature" on systems like Ubuntu and Debian.<br><br>apt-get installs a cron script at /etc/cron.d/php5 that checks the session.gc_maxlifetime variable and then deletes all old sessions every 9 and 39 minutes.<br><br>The problem is: If you set the maxlifetime for a specific virtual host, those settings will be ignored. Lets say you want your server to store sessions for only 30 minutes, but for one special website you want all sessions to be 24 hours. If you set the session.gc_maxlifetime in .htaccess, your apache conf or use ini_set in your code, it won&apos;t work and sessions will still be destroyed after 30 minutes. That&apos;s because /usr/lib/php5/maxlifetime (found in that cron file) will always return the value in your php.ini, not the values you set in .htaccess.<br><br>A workaround is to set the maxlifetime to the maximum your sites require, and then configure a shorter maxlifetime in your .htaccess for those sites that don&apos;t need it. <br><br>Another solution is to give the php5 file in /etc/cron.d sane values, ie, only let it run at 3am in the morning, but you&apos;ll have to remember to block the replacement of this file it every time you update php.  

---

when setting the path that the cookie is valid for, always remember to have that trailing &apos;/&apos;.<br><br>CORRECT:<br>session_set_cookie_params (0, &apos;/yourpath/&apos;);<br><br>INCORRECT:<br>session_set_cookie_params (0, &apos;/yourpath&apos;);<br><br>no comment on how long it took me to realize that this was the cause of my authentication/session problems...  

---

REMEMBER, that if you have a multi-subdomain site, you must put the following to enable a session id on the whole website:<br><br>

```
<?php
session_set_cookie_params(0, '/', '.example.com');
session_start();
?>
```
<br><br>Otherwise, you&apos;ll have 2 diffrent sessions on e.g. news.example.com and download.example.com  

---

[Official documentation page](https://www.php.net/manual/en/function.session-set-cookie-params.php)

**[To root](/README.md)**