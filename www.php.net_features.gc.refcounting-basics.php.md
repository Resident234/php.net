# Reference Counting Basics




<div class="phpcode"><span class="html">
If a variable is not present in the current scope xdebug_debug_zval&#x3000;will return null.</span>
</div>
  

#


<div class="phpcode"><span class="html">
There seems to be no way to inspect the reference count of a specific class variable but you can view the reference count of all variables in the current class instance with xdebug_debug_zval(&apos;this&apos;);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.gc.refcounting-basics.php)

**[To root](/README.md)**