# $GLOBALS



As of PHP 5.4 $GLOBALS is now initialized just-in-time. This means there now is an advantage to not use the $GLOBALS variable as you can avoid the overhead of initializing it. How much of an advantage that is I&apos;m not sure, but I&apos;ve never liked $GLOBALS much anyways.  

#

Watch out when you are trying to set $GLOBALS to the local variable.<br><br>Even without reference operator "&amp;" your variable seems to be referenced to the $GLOBALS<br><br>You can test this behaviour using below code<br><br>

```
<?php<br>/**<br> * Result:<br> * POST: B, Variable: C<br> * GLOBALS: C, Variable: C<br> */<br> <br>// Testing $_POST<br>$_POST[&apos;A&apos;] = &apos;B&apos;;<br> <br>$nonReferencedPostVar = $_POST;<br>$nonReferencedPostVar[&apos;A&apos;] = &apos;C&apos;;<br> <br>echo &apos;POST: &apos;.$_POST[&apos;A&apos;].&apos;, Variable: &apos;.$nonReferencedPostVar[&apos;A&apos;]."\n\n";<br> <br>// Testing Globals<br>$GLOBALS[&apos;A&apos;] = &apos;B&apos;;<br> <br>$nonReferencedGlobalsVar = $GLOBALS;<br>$nonReferencedGlobalsVar[&apos;A&apos;] = &apos;C&apos;;<br> <br>echo &apos;GLOBALS: &apos;.$GLOBALS[&apos;A&apos;].&apos;, Variable: &apos;.$nonReferencedGlobalsVar[&apos;A&apos;]."\n\n";  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.globals.php)

**[To root](/README.md)**