# Memcached::increment




<div class="phpcode"><span class="html">
Spent a long time frustrated with this.&#xA0; If you read the patch notes carefully:<br><br>- Make increment/decrement initialize value when it is not available (when using binary protocol).<br><br>If you dont have the opt binary protocol set the arguments for initial value just return an error 38 - INVALID ARGUMENTS. This is not documented.</span>
</div>
  

#


<div class="phpcode"><span class="html">
increment does not alter the time to live of the object.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.increment.php)

**[To root](/README.md)**