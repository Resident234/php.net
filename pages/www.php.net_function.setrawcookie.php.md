# setrawcookie



Firefox is following the real spec and does not decode &apos;+&apos; to space...in fact it further encodes them to &apos;%2B&apos; to store the cookie.  If you read a cookie using javascript and unescape it, all your spaces will be turned to &apos;+&apos;.<br>To fix this problem, use setrawcookie and rawurlencode:<br><br>

```
<?php
setrawcookie('cookie_name', rawurlencode($value), time()+60*60*24*365);
?>
```
<br><br>The only change is that spaces will be encoded to &apos;%20&apos; instead of &apos;+&apos; and will now decode properly.  

---

setrawcookie() isn&apos;t entirely &apos;raw&apos;. It will check the value for invalid characters, and then disallow the cookie if there are any. These are the invalid characters to keep in mind: &apos;,;&lt;space&gt;\t\r\n\013\014&apos;.<br><br>Note that comma, space and tab are three of the invalid characters. IE, Firefox and Opera work fine with these characters, and PHP reads cookies containing them fine as well. However, if you want to use these characters in cookies that you set from php, you need to use header().  

---

[Official documentation page](https://www.php.net/manual/en/function.setrawcookie.php)

**[To root](/README.md)**