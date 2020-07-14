# $GLOBALS



As of PHP 5.4 $GLOBALS is now initialized just-in-time. This means there now is an advantage to not use the $GLOBALS variable as you can avoid the overhead of initializing it. How much of an advantage that is I&apos;m not sure, but I&apos;ve never liked $GLOBALS much anyways.  

#

Watch out when you are trying to set $GLOBALS to the local variable.<br><br>Even without reference operator "&amp;" your variable seems to be referenced to the $GLOBALS<br><br>You can test this behaviour using below code<br><br>

```
<?php
/**
 * Result:
 * POST: B, Variable: C
 * GLOBALS: C, Variable: C
 */
 
// Testing $_POST
$_POST['A'] = 'B';
 
$nonReferencedPostVar = $_POST;
$nonReferencedPostVar['A'] = 'C';
 
echo 'POST: '.$_POST['A'].', Variable: '.$nonReferencedPostVar['A']."\n\n";
 
// Testing Globals
$GLOBALS['A'] = 'B';
 
$nonReferencedGlobalsVar = $GLOBALS;
$nonReferencedGlobalsVar['A'] = 'C';
 
echo 'GLOBALS: '.$GLOBALS['A'].', Variable: '.$nonReferencedGlobalsVar['A']."\n\n";?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.globals.php)

**[To root](/README.md)**