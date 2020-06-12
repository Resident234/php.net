# Deprecated features in PHP 7.2.x




<div class="phpcode"><span class="html">
Instead of __autoload(), you can use spl_autoload_register() very easily, as per the documentation:<br><br>spl_autoload_register(function ($class) {<br>&#xA0; &#xA0; include &apos;classes/&apos; . $class . &apos;.class.php&apos;;<br>});<br><br>And this lets you have multiple autoloaders instead of one global one.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration72.deprecated.php)

**[⬆ to root](/)**