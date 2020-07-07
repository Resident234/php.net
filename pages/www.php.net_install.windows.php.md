# Installation on Windows systems





If you make changes to your PHP.ini file, consider the following.



(I&apos;m running IIS5 on W2K server. I don&apos;t know about 2K3)



PHP will not &quot;take&quot; the changes until the webserver is restarted, and that doesn&apos;t mean through the MMC.&#xA0; Usually folks just reboot. But you can also use the following commands, for a much faster &quot;turnaround&quot;.&#xA0; At a command line prompt, type:



iisreset /stop



and that will stop the webserver service.&#xA0; Then type:



net start w3svc



and that will start the webserver service again.&#xA0; MUCH faster than a reboot, and you can check your changes faster as a result with the old:





```
<?php&gt;

phpinfo();

?>
```




in your page somewhere.



I wish I could remember where I read this tip; it isn&apos;t anything I came up with...

  

#



You can have multiple versions of PHP running on the same Apache server. I have seen many different solutions pointing at achieving this, but most of them required installing additional instances of Apache, redirecting ports/hosts, etc., which was not satisfying for me.
Finally, I have come up with the simplest solution I&apos;ve seen so far, limited to reconfiguring Apache&apos;s httpd.conf.

My goal is to have PHP5 as the default scripting language for .php files in my DocumentRoot (which is in my case d:/htdocs), and PHP4 for specified DocumentRoot subdirectories.

Here it is (Apache&apos;s httpd.conf contents):

---------------------------
# replace with your PHP4 directory
ScriptAlias /php4/ &quot;c:/usr/php4/&quot;
# replace with your PHP5 directory
ScriptAlias /php5/ &quot;c:/usr/php5/&quot;

AddType application/x-httpd-php .php
Action application/x-httpd-php &quot;/php5/php-cgi.exe&quot;

# populate this for every directory with PHP4 code
&lt;Directory &quot;d:/htdocs/some_subdir&quot;&gt;
&#xA0; &#xA0; Action application/x-httpd-php &quot;/php4/php.exe&quot;
&#xA0; &#xA0; # directory where your PHP4 php.ini file is located at
&#xA0; &#xA0; SetEnv PHPRC &quot;c:/usr/php4&quot;
&lt;/Directory&gt;

# remember to put this section below the above
&lt;Directory &quot;d:/htdocs&quot;&gt;
&#xA0; &#xA0; # directory where your PHP5 php.ini file is located at
&#xA0; &#xA0; SetEnv PHPRC &quot;c:/usr/php5&quot;
&lt;/Directory&gt;
---------------------------

This solution is not limited to having only two parallel versions of PHP. You can play with httpd.conf contents to have as many PHP versions configured as you want.
You can also use multiple php.ini configuration files for the same PHP version (but for different DocumentRoot subfolders), which might be useful in some cases.

Remember to put your php.ini files in directories specified in lines &quot;SetEnv PHPRC...&quot;, and make sure that there&apos;s no php.ini files in other directories (such as c:\windows in Windows).

And finally, as you can see, I run PHP in CGI mode. This has its advantages and limitations. If you have to run PHP as Apache module, then... sorry - you have to use other solution (the best advice as always is: Google it!).

Hope this helps someone.

  

#

[Official documentation page](https://www.php.net/manual/en/install.windows.php)

**[To root](/README.md)**