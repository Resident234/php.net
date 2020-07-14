# headers_list



This function won&apos;t work for when you&apos;re running PHP from the command line. If will always return an empty array. This can be an issue when testing your project using PHPUnit or Codeception.<br><br>To solve this, install the xdebug extension and use `xdebug_get_headers` when on the cli.<br><br>

```
<?php
$headers = php_sapi_name() === 'cli' ? xdebug_get_headers() : headers_list();
?>
```
  

#

note that it does not return the status header<br><br>

```
<?php

header('HTTP/1.1 301 Moved Permanently', true, 301);

header('foo: bar');
header('a: b');
header('colon less example');

print_r(headers_list());
?>
```
<br><br>Array<br>(<br>    [0] =&gt; X-Powered-By: PHP/5.4.7<br>    [1] =&gt; foo: bar<br>    [2] =&gt; a: b<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.headers-list.php)

**[To root](/README.md)**