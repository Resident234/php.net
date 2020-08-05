# setcookie



Instead of this:<br>

```
<?php setcookie( "TestCookie", $value, time()+(60*60*24*30) ); ?>
```


You can this:


```
<?php setcookie( "TestCookie", $value, strtotime( '+30 days' ) ); ?>
```
  

---

Want to remove a cookie?<br><br>Many people do it the complicated way:<br>setcookie(&apos;name&apos;, &apos;content&apos;, time()-3600);<br><br>But why do you make it so complicated and risk it not working, when the client&apos;s time is wrong? Why fiddle around with time();<br><br>Here&apos;s the easiest way to unset a cookie:<br>setcookie(&apos;name&apos;, &apos;content&apos;, 1);<br><br>Thats it.  

---

Note when setting "array cookies" that a separate cookie is set for each element of the array.<br><br>On high traffic sites, this can substantially increase the size of subsequent HTTP requests from clients (including requests for static content on the same domain).<br><br>More importantly though, the cookie specification says that browsers need only accept 20 cookies per domain.  This limit is increased to 50 by Firefox, and to 30 by Opera, but IE6 and IE7 enforce the limit of 20 cookie per domain.  Any cookies beyond this limit will either knock out an older cookie or be ignored/rejected by the browser.  

---

It&apos;s worth a mention: you should avoid dots on cookie names.<br><br>

```
<?php
// this will actually set 'ace_fontSize' name:
setcookie( 'ace.fontSize', 18 );
?>
```
  

---

something that wasn&apos;t made clear to me here and totally confused me for a while was that domain names must contain at least two dots (.), hence &apos;localhost&apos; is invalid and the browser will refuse to set the cookie! instead for localhost you should use false.<br><br>to make your code work on both localhost and a proper domain, you can do this:<br><br>

```
<?php

$domain = ($_SERVER['HTTP_HOST'] != 'localhost') ? $_SERVER['HTTP_HOST'] : false;
setcookie('cookiename', 'data', time()+60*60*24*365, '/', $domain, false);

?>
```
  

---

if you are having problems seeing cookies sometimes or deleting cookies sometimes, despite following the advice below, make sure you are setting the cookie with the domain argument. Set it with the dot before the domain as the examples show: ".example.com".  I wasn&apos;t specifying the domain, and finally realized I was setting the cookie when the browser url had the http://www.example.com and later trying to delete it when the url didn&apos;t have the www. ie. http://example.com. This also caused the page to be unable to find the cookie when the www. wasn&apos;t in the domain.  (When you add the domain argument to the setcookie code that creates the cookie, make sure you also add it to the code that deletes the cookie.)  

---

If you&apos;re having problem with IE not accepting session cookies this could help:<br><br>It seems the IE (6, 7, 8 and 9) do not accept the part &apos;Expire=0&apos; when setting a session cookie. To fix it just don&apos;t put any expire at all. The default behavior when the &apos;Expire&apos; is not set is to set the cookie as a session one. <br><br>(Firefox doesn&apos;t complains, btw.)  

---

If you want to delete all cookies on your domain, you may want to use the value of:<br><br>

```
<?php $_SERVER['HTTP_COOKIE'] ?>
```


rather than:



```
<?php $_COOKIE ?>
```


to dertermine the cookie names. 
If cookie names are in Array notation, eg: user[username] 
Then PHP will automatically create a corresponding array in $_COOKIE. Instead use $_SERVER['HTTP_COOKIE'] as it mirrors the actual HTTP Request header. 



```
<?php

// unset cookies
if (isset($_SERVER['HTTP_COOKIE'])) {
    $cookies = explode(';', $_SERVER['HTTP_COOKIE']);
    foreach($cookies as $cookie) {
        $parts = explode('=', $cookie);
        $name = trim($parts[0]);
        setcookie($name, '', time()-1000);
        setcookie($name, '', time()-1000, '/');
    }
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.setcookie.php)

**[To root](/README.md)**