# Memcached::decrement



decrement will not change TTL of the stored key/value.  

#

Found possible bug :<br>decrement fails and returns -1  when memcached::OPT_BINARY_PROTOCOL is set to true.<br><br>tested on PECL Memcached 2.1.0 and libmemcached version 1.0.8  

#

[Official documentation page](https://www.php.net/manual/en/memcached.decrement.php)

**[To root](/README.md)**