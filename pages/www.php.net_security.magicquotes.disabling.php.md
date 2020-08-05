# Disabling Magic Quotes



I have discovered that my host doesn&apos;t like either of the following directives in the .htaccess file:<br><br>php_flag magic_quotes_gpc Off<br>php_value magic_quotes_gpc Off<br><br>However, there is another way to disable this setting even if you don&apos;t have access to the server configuration - you can put a php.ini file in the directory where your scripts are with the directive:<br><br>magic_quotes_gpc = Off<br><br>However, these does not propogate unlike  .htaccess rules, so if you launch from a sub-directory, you need the php.ini file in each directory you have as script entry points.  

---

Here&apos;s a couple tips when using scripts on different (often shared) hosts, where ini_set doesn&apos;t work and php directives in .htaccess causes a 500 Internal Server Error.<br><br>Firstly, copy the server&apos;s php.ini file to your domain&apos;s web-root folder. To find the correct paths, use phpinfo() and look for "Configuration File (php.ini) Path" and "DOCUMENT_ROOT"<br><br>It&apos;s unlikely you&apos;ll have access to the php.ini via FTP, so instead run a script with a simple copy command (obviously inserting your paths):<br><br>exec("cp /usr/local/php/etc/php.ini /home/LinuxPackage/public_html/php.ini);<br><br>Edit the now-accessible php.ini file, and add settings like &apos;magic_quotes_gpc = off&apos; at the bottom (regardless of whether they&apos;ve been set earlier in the file). I also set:<br><br>[PHP]<br>max_execution_time = 60<br>max_input_time = 90<br>memory_limit = 64M<br>post_max_size = 32M<br>upload_max_filesize = 31M<br>magic_quotes_gpc = Off<br><br>Finally add the below line to your web-root htaccess file, to make the local php.ini the web-root default (so you don&apos;t need a copy in every script sub-folder):<br><br>SetEnv PHPRC /home/LinuxPackage/public_html/php.ini<br><br>Hope that helps a few people save some time!<br><br>Mike.<br><br>P.S. Using the new php_ini_loaded_file() function the whole lot could be done in three lines:<br><br>exec("cp " . php_ini_loaded_file() . " " . $_SERVER[&apos;DOCUMENT_ROOT&apos;] . "/php.ini");<br>fwrite(fopen("{$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/php.ini", &apos;a&apos;), PHP_EOL . &apos;[PHP]&apos; . PHP_EOL . "magic_quotes_gpc = Off" . PHP_EOL);<br>fwrite(fopen("{$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/.htaccess", &apos;a&apos;), PHP_EOL . "SetEnv PHPRC {$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/php.ini" . PHP_EOL);  

---

A php5 way:<br><br>

```
<?php
if (get_magic_quotes_gpc()) {
    function stripslashes_gpc(&amp;$value)
    {
        $value = stripslashes($value);
    }
    array_walk_recursive($_GET, 'stripslashes_gpc');
    array_walk_recursive($_POST, 'stripslashes_gpc');
    array_walk_recursive($_COOKIE, 'stripslashes_gpc');
    array_walk_recursive($_REQUEST, 'stripslashes_gpc');
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/security.magicquotes.disabling.php)

**[To root](/README.md)**