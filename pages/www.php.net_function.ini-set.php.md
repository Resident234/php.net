# ini_set



I have experienced on some systems that ini_set() will fail and return a false, when trying to set a setting that was set inside php.ini inside a per-host setting. Beware of this.  

#

When checking for the success of ini_set(), keep in mind that it will return the OLD value upon success - which may have been "0". Therefore you cannot just compare the return value - you have to check for "identity":<br><br>

```
<?php

// This will NOT determine the success of ini_set(), instead
// it only tests if the old value had been equivalent to false
if ( !ini_set( &apos;display_errors&apos;, &apos;1&apos; ) ) 
    throw new \Exception( &apos;Unable to set display_errors.&apos; );

// This is the CORRECT way to determine success
if ( ini_set( &apos;display_errors&apos;, &apos;1&apos; ) === false ) 
    throw new \Exception( &apos;Unable to set display_errors.&apos; );    

?>
```
<br><br>This explains reported situations where ini_set() "always" seems to fail!  

#

[[[Editors note: Yes, this is very true.  Same with <br>register_globals, magic_quotes_gpc and others.<br>]]]<br><br>Many settings, although they do get set, have no influence in your script.... like upload_max_filesize will get set but uploaded files are already passed to your PHP script before the settings are changed.<br><br>Also other settings, set by ini_set(), may be to late because of this (post_max_size etc.).<br>beware, try settings thru php.ini or .htaccess.  

#

Be careful with setting an output_handler, as you can&apos;t use ini_set() to change it. *sigh*<br><br>In my php.ini I have this for my web pages (and I want it): <br><br>  output_handler = ob_gzhandler<br><br>But this causes my command line scripts to not show output until the very end.<br><br>#!/usr/bin/php -q<br>

```
<?php
ini_set(&apos;output_handler&apos;, &apos;mb_output_handler&apos;);
echo "\noutput_handler =&gt; " . ini_get(&apos;output_handler&apos;) . "\n";
?>
```


root@# ./myscript.php<br>output_handler =&gt; ob_gzhandler<br><br>Apparently (acording to Richard Lynch):<br><br>&gt; TOO LATE!<br>&gt; The ob_start() has already kicked in by this point.<br>&gt; ob_flush() until there are no more buffers.  

#

set PHP_INI_PERDIR settings in a .htaccess file with &apos;php_flag&apos; like this:<br><br>php_flag register_globals off
php_flag magic_quotes_gpc on?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.ini-set.php)

**[To root](/README.md)**