# $GLOBALS





As of PHP 5.4 $GLOBALS is now initialized just-in-time. This means there now is an advantage to not use the $GLOBALS variable as you can avoid the overhead of initializing it. How much of an advantage that is I&apos;m not sure, but I&apos;ve never liked $GLOBALS much anyways.

  

#



Watch out when you are trying to set $GLOBALS to the local variable.

Even without reference operator &quot;&amp;&quot; your variable seems to be referenced to the $GLOBALS

You can test this behaviour using below code



```
<?php
/**
 * Result:
 * POST: B, Variable: C
 * GLOBALS: C, Variable: C
 */
 
// Testing $_POST
$_POST[&apos;A&apos;] = &apos;B&apos;;
 
$nonReferencedPostVar = $_POST;
$nonReferencedPostVar[&apos;A&apos;] = &apos;C&apos;;
 
echo &apos;POST: &apos;.$_POST[&apos;A&apos;].&apos;, Variable: &apos;.$nonReferencedPostVar[&apos;A&apos;].&quot;\n\n&quot;;
 
// Testing Globals
$GLOBALS[&apos;A&apos;] = &apos;B&apos;;
 
$nonReferencedGlobalsVar = $GLOBALS;
$nonReferencedGlobalsVar[&apos;A&apos;] = &apos;C&apos;;
 
echo &apos;GLOBALS: &apos;.$GLOBALS[&apos;A&apos;].&apos;, Variable: &apos;.$nonReferencedGlobalsVar[&apos;A&apos;].&quot;\n\n&quot;;


  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.globals.php)

**[To root](/README.md)**