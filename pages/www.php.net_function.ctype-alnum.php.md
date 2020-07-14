# ctype_alnum



ctype_alnum() is a godsend for quick and easy username/data filtering when used in conjunction with str_replace().<br><br>Let&apos;s say your usernames have dash(-) and underscore(_) allowable and alphanumeric digits as well.<br><br>Instead of a regex you can trade a bit of performance for simplicity:<br><br>

```
<?php
$sUser = &apos;my_username01&apos;;
$aValid = array(&apos;-&apos;, &apos;_&apos;);

if(!ctype_alnum(str_replace($aValid, &apos;&apos;, $sUser))) {
    echo &apos;Your username is not properly formatted.&apos;;
}
?>
```
  

#

Quicktip: If ctype is not enabled by default on your server, replace ctype_alnum($var) with preg_match(&apos;/^[a-zA-Z0-9]+$/&apos;, $var).  

#

[Official documentation page](https://www.php.net/manual/en/function.ctype-alnum.php)

**[To root](/README.md)**