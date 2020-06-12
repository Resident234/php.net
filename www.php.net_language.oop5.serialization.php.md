# Object Serialization




<div class="phpcode"><span class="html">
Note that static members of an object are not serialized.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Reading this page you&apos;d be left with the impression that a class&apos;s `serialize` and `unserialize` methods are unrelated to the `serialize` and `unserialize` core functions; that only `__sleep` and `__unsleep` allow you to customize an object&apos;s serialization interface. But look at <a href="http://php.net/manual/en/class.serializable.php" rel="nofollow" target="_blank">http://php.net/manual/en/class.serializable.php</a> and you&apos;ll see that there is a more straightforward way to control how a user-defined object is serialized and unserialized.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.serialization.php)

**[⬆ to root](/)**