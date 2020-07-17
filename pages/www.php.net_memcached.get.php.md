# Memcached::get



As of some version of php7 (i was not able to determine which exactly).<br>The $cas_token is no longer valid argument. It has been removed in favor of flags argument, as it appears to be causing issues when subclassing the Memcached class.<br><br>See https://github.com/php-memcached-dev/php-memcached/pull/214 for more details.<br><br>Basically you need to <br>

```
<?php
function memcacheGet($key, $cb = null, &amp;$cas = null) {
  $m = memcacheGetObject();
  if(empty($m))
    return false;
  if(defined('Memcached::GET_EXTENDED')) {
    //Incompatible change in php7, took me 2 hours to figure this out, grrr
    $_o = $m->get($key, $cb, Memcached::GET_EXTENDED);
    $o = $_o['value'];
    $cas = $_o['cas'];
  } else {
    $o = $m->get($key, $cb, $cas);
  }
  return $o;
}
?>
```
  

#

This method also returns false in case you set the value to false, so in order to have a proper fault mechanism in place you need to check the result code to be certain that a key really does not exist in memcached.<br><br>

```
<?php
$Memcached = new Memcached();
$Memcached->addServer('localhost', 11211);
$Memcached->set('key', false);
var_dump($Memcached->get('key'));       // boolean false
var_dump($Memcached->getResultCode());  // int 0 which is Memcached::RES_SUCCESS
?>
```
<br><br>Or just make sure the values are not false :)  

#

[Official documentation page](https://www.php.net/manual/en/memcached.get.php)

**[To root](/README.md)**