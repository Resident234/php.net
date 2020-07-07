# $_REQUEST





Don&apos;t forget, because $_REQUEST is a different variable than $_GET and $_POST, it is treated as such in PHP -- modifying $_GET or $_POST elements at runtime will not affect the ellements in $_REQUEST, nor vice versa.

e.g:



```
<?php

$_GET[&apos;foo&apos;] = &apos;a&apos;;
$_POST[&apos;bar&apos;] = &apos;b&apos;;
var_dump($_GET); // Element &apos;foo&apos; is string(1) &quot;a&quot;
var_dump($_POST); // Element &apos;bar&apos; is string(1) &quot;b&quot;
var_dump($_REQUEST); // Does not contain elements &apos;foo&apos; or &apos;bar&apos;

?>
```


If you want to evaluate $_GET and $_POST variables by a single token without including $_COOKIE in the mix, use&#xA0; $_SERVER[&apos;REQUEST_METHOD&apos;] to identify the method used and set up a switch block accordingly, e.g:



```
<?php

switch($_SERVER[&apos;REQUEST_METHOD&apos;])
{
case &apos;GET&apos;: $the_request = &amp;$_GET; break;
case &apos;POST&apos;: $the_request = &amp;$_POST; break;
.
. // Etc.
.
default:
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.request.php)

**[To root](/README.md)**