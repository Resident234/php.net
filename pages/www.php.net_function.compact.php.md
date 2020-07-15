# compact



Can also handy for debugging, to quickly show a bunch of variables and their values:<br><br>

```
<?php
print_r(compact(explode(' ', 'count acw cols coldepth')));
?>
```
<br><br>gives<br><br>Array<br>(<br>    [count] =&gt; 70<br>    [acw] =&gt; 9<br>    [cols] =&gt; 7<br>    [coldepth] =&gt; 10<br>)  

#

So compact(&apos;var1&apos;, &apos;var2&apos;) is the same as saying array(&apos;var1&apos; =&gt; $var1, &apos;var2&apos; =&gt; $var2) as long as $var1 and $var2 are set.  

#

The description says that compact is the opposite of extract() but it is important to understand that it does not completely reverse extract().  In particluar compact() does not unset() the argument variables given to it (and that extract() may have created).  If you want the individual variables to be unset after they are combined into an array then you have to do that yourself.  

#

[Official documentation page](https://www.php.net/manual/en/function.compact.php)

**[To root](/README.md)**