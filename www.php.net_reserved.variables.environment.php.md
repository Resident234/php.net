# $_ENV




<div class="phpcode"><span class="html">
If your $_ENV array is mysteriously empty, but you still see the variables when calling getenv() or in your phpinfo(), check your <a href="http://us.php.net/manual/en/ini.core.php#ini.variables-order" rel="nofollow" target="_blank">http://us.php.net/manual/en/ini.core.php#ini.variables-order</a> ini setting to ensure it includes &quot;E&quot; in the string.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Comments for this page seem to indicate getenv() returns environment variables in all cases.<br><br>For getenv() to work, php.ini variables_order must contain &apos;E&apos;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.environment.php)

**[To root](/)**