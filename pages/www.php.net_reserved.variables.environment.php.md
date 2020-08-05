# $_ENV



If your $_ENV array is mysteriously empty, but you still see the variables when calling getenv() or in your phpinfo(), check your http://us.php.net/manual/en/ini.core.php#ini.variables-order ini setting to ensure it includes "E" in the string.  

---

Comments for this page seem to indicate getenv() returns environment variables in all cases.<br><br>For getenv() to work, php.ini variables_order must contain &apos;E&apos;.  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.environment.php)

**[To root](/README.md)**