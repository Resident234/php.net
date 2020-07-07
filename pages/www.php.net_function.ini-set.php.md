# ini_set





I have experienced on some systems that ini_set() will fail and return a false, when trying to set a setting that was set inside php.ini inside a per-host setting. Beware of this.

  

#



When checking for the success of ini_set(), keep in mind that it will return the OLD value upon success - which may have been &quot;0&quot;. Therefore you cannot just compare the return value - you have to check for &quot;identity&quot;:



```
<?php

// This will NOT determine the success of ini_set(), instead
// it only tests if the old value had been equivalent to false
if ( !ini_set( &apos;display_errors&apos;, &apos;1&apos; ) ) 
&#xA0; &#xA0; throw new \Exception( &apos;Unable to set display_errors.&apos; );

// This is the CORRECT way to determine success
if ( ini_set( &apos;display_errors&apos;, &apos;1&apos; ) === false ) 
&#xA0; &#xA0; throw new \Exception( &apos;Unable to set display_errors.&apos; );&#xA0; &#xA0; 

php?>
```


This explains reported situations where ini_set() &quot;always&quot; seems to fail!

  

#



[[[Editors note: Yes, this is very true.&#xA0; Same with 

register_globals, magic_quotes_gpc and others.

]]]



Many settings, although they do get set, have no influence in your script.... like upload_max_filesize will get set but uploaded files are already passed to your PHP script before the settings are changed.



Also other settings, set by ini_set(), may be to late because of this (post_max_size etc.).

beware, try settings thru php.ini or .htaccess.

  

#



Be careful with setting an output_handler, as you can&apos;t use ini_set() to change it. *sigh*

In my php.ini I have this for my web pages (and I want it): 

&#xA0; output_handler = ob_gzhandler

But this causes my command line scripts to not show output until the very end.

#!/usr/bin/php -q


```
<?php
ini_set(&apos;output_handler&apos;, &apos;mb_output_handler&apos;);
echo &quot;\noutput_handler =&gt; &quot; . ini_get(&apos;output_handler&apos;) . &quot;\n&quot;;
php?>
```


root@# ./myscript.php
output_handler =&gt; ob_gzhandler

Apparently (acording to Richard Lynch):

&gt; TOO LATE!
&gt; The ob_start() has already kicked in by this point.
&gt; ob_flush() until there are no more buffers.

  

#



set PHP_INI_PERDIR settings in a .htaccess file with &apos;php_flag&apos; like this:

php_flag register_globals off
php_flag magic_quotes_gpc on

  

#

[Official documentation page](https://www.php.net/manual/en/function.ini-set.php)

**[To root](/README.md)**