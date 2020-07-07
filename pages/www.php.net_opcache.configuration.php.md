# Runtime Configuration





When using PHP on a windows platform and enabling opcache, you might run into occasional 500 errors. These will appear to show up entirely random.

When this happens, your windows Event log (Windows Logs/Application) will show (probably multiple)&#xA0; entries from Zend OPcache with Event ID 487. Further information will state the following error message: &quot;Base address marks unusable memory region&quot;.

This issue can be resolved by adding the following to your php.ini:

&#xA0; &#xA0; opcache.mmap_base = 0x20000000

Unfortunately I do not know the significance of the value &quot;0x20000000&quot;. I can only tell you that this value works to solve the problem (Tried and tested)

  

#

[Official documentation page](https://www.php.net/manual/en/opcache.configuration.php)

**[To root](/README.md)**