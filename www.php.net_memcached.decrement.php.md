# Memcached::decrement




<div class="phpcode"><span class="html">
decrement will not change TTL of the stored key/value.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Found possible bug :<br>decrement fails and returns -1&#xA0; when memcached::OPT_BINARY_PROTOCOL is set to true.<br><br>tested on PECL Memcached 2.1.0 and libmemcached version 1.0.8</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.decrement.php)

**[⬆ to root](/)**