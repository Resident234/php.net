# putenv



putenv/getenv, $_ENV, and phpinfo(INFO_ENVIRONMENT) are three completely distinct environment stores. doing putenv("x=y") does not affect $_ENV; but also doing $_ENV["x"]="y" likewise does not affect getenv("x"). And neither affect what is returned in phpinfo().<br><br>Assuming the USER environment variable is defined as "dave" before running the following:<br><br>

```
<?php
print "env is: ".$_ENV["USER"]."\n";
print "(doing: putenv fred)\n";
putenv("USER=fred");
print "env is: ".$_ENV["USER"]."\n";
print "getenv is: ".getenv("USER")."\n";
print "(doing: set _env barney)\n";
$_ENV["USER"]="barney";
print "getenv is: ".getenv("USER")."\n";
print "env is: ".$_ENV["USER"]."\n";
phpinfo(INFO_ENVIRONMENT);
?>
```
<br><br>prints:<br><br>env is: dave<br>(doing: putenv fred)<br>env is: dave<br>getenv is: fred<br>(doing: set _env barney)<br>getenv is: fred<br>env is: barney<br>phpinfo()<br><br>Environment<br><br>Variable =&gt; Value<br>...<br>USER =&gt; dave<br>...  

#

The other problem with the code from av01 at bugfix dot cc is that<br>the behaviour is as per the comments here, not there:<br>

```
<?php
putenv('MYVAR='); // set MYVAR to an empty value.  It is in the environment
putenv('MYVAR'); // unset MYVAR.  It is removed from the environment
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.putenv.php)

**[To root](/README.md)**