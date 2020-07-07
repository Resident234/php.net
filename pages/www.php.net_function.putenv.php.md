# putenv





putenv/getenv, $_ENV, and phpinfo(INFO_ENVIRONMENT) are three completely distinct environment stores. doing putenv(&quot;x=y&quot;) does not affect $_ENV; but also doing $_ENV[&quot;x&quot;]=&quot;y&quot; likewise does not affect getenv(&quot;x&quot;). And neither affect what is returned in phpinfo().

Assuming the USER environment variable is defined as &quot;dave&quot; before running the following:



```
<?php
print &quot;env is: &quot;.$_ENV[&quot;USER&quot;].&quot;\n&quot;;
print &quot;(doing: putenv fred)\n&quot;;
putenv(&quot;USER=fred&quot;);
print &quot;env is: &quot;.$_ENV[&quot;USER&quot;].&quot;\n&quot;;
print &quot;getenv is: &quot;.getenv(&quot;USER&quot;).&quot;\n&quot;;
print &quot;(doing: set _env barney)\n&quot;;
$_ENV[&quot;USER&quot;]=&quot;barney&quot;;
print &quot;getenv is: &quot;.getenv(&quot;USER&quot;).&quot;\n&quot;;
print &quot;env is: &quot;.$_ENV[&quot;USER&quot;].&quot;\n&quot;;
phpinfo(INFO_ENVIRONMENT);
php?>
```


prints:

env is: dave
(doing: putenv fred)
env is: dave
getenv is: fred
(doing: set _env barney)
getenv is: fred
env is: barney
phpinfo()

Environment

Variable =&gt; Value
...
USER =&gt; dave
...

  

#



The other problem with the code from av01 at bugfix dot cc is that
the behaviour is as per the comments here, not there:


```
<?php
putenv(&apos;MYVAR=&apos;); // set MYVAR to an empty value.&#xA0; It is in the environment
putenv(&apos;MYVAR&apos;); // unset MYVAR.&#xA0; It is removed from the environment
php?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.putenv.php)

**[To root](/README.md)**