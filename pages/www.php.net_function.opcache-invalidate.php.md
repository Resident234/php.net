# opcache_invalidate





Beware that only existing files can be invalidated.

Instead of removing a file from opcache that you have delete, you need to call opcache_invalidate before deleting it.

  

#

[Official documentation page](https://www.php.net/manual/en/function.opcache-invalidate.php)

**[To root](/README.md)**