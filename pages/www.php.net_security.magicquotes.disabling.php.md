# Disabling Magic Quotes





I have discovered that my host doesn&apos;t like either of the following directives in the .htaccess file:

php_flag magic_quotes_gpc Off
php_value magic_quotes_gpc Off

However, there is another way to disable this setting even if you don&apos;t have access to the server configuration - you can put a php.ini file in the directory where your scripts are with the directive:

magic_quotes_gpc = Off

However, these does not propogate unlike&#xA0; .htaccess rules, so if you launch from a sub-directory, you need the php.ini file in each directory you have as script entry points.

  

#



Here&apos;s a couple tips when using scripts on different (often shared) hosts, where ini_set doesn&apos;t work and php directives in .htaccess causes a 500 Internal Server Error.

Firstly, copy the server&apos;s php.ini file to your domain&apos;s web-root folder. To find the correct paths, use phpinfo() and look for &quot;Configuration File (php.ini) Path&quot; and &quot;DOCUMENT_ROOT&quot;

It&apos;s unlikely you&apos;ll have access to the php.ini via FTP, so instead run a script with a simple copy command (obviously inserting your paths):

exec(&quot;cp /usr/local/php/etc/php.ini /home/LinuxPackage/public_html/php.ini);

Edit the now-accessible php.ini file, and add settings like &apos;magic_quotes_gpc = off&apos; at the bottom (regardless of whether they&apos;ve been set earlier in the file). I also set:

[PHP]
max_execution_time = 60
max_input_time = 90
memory_limit = 64M
post_max_size = 32M
upload_max_filesize = 31M
magic_quotes_gpc = Off

Finally add the below line to your web-root htaccess file, to make the local php.ini the web-root default (so you don&apos;t need a copy in every script sub-folder):

SetEnv PHPRC /home/LinuxPackage/public_html/php.ini

Hope that helps a few people save some time!

Mike.

P.S. Using the new php_ini_loaded_file() function the whole lot could be done in three lines:

exec(&quot;cp &quot; . php_ini_loaded_file() . &quot; &quot; . $_SERVER[&apos;DOCUMENT_ROOT&apos;] . &quot;/php.ini&quot;);
fwrite(fopen(&quot;{$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/php.ini&quot;, &apos;a&apos;), PHP_EOL . &apos;[PHP]&apos; . PHP_EOL . &quot;magic_quotes_gpc = Off&quot; . PHP_EOL);
fwrite(fopen(&quot;{$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/.htaccess&quot;, &apos;a&apos;), PHP_EOL . &quot;SetEnv PHPRC {$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/php.ini&quot; . PHP_EOL);

  

#



A php5 way:



```
<?php
if (get_magic_quotes_gpc()) {
&#xA0; &#xA0; function stripslashes_gpc(&amp;$value)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $value = stripslashes($value);
&#xA0; &#xA0; }
&#xA0; &#xA0; array_walk_recursive($_GET, &apos;stripslashes_gpc&apos;);
&#xA0; &#xA0; array_walk_recursive($_POST, &apos;stripslashes_gpc&apos;);
&#xA0; &#xA0; array_walk_recursive($_COOKIE, &apos;stripslashes_gpc&apos;);
&#xA0; &#xA0; array_walk_recursive($_REQUEST, &apos;stripslashes_gpc&apos;);
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/security.magicquotes.disabling.php)

**[To root](/README.md)**