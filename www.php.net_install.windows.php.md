# Installation on Windows systems




<div class="phpcode"><span class="html">
If you make changes to your PHP.ini file, consider the following.
<br>
<br>(I&apos;m running IIS5 on W2K server. I don&apos;t know about 2K3)
<br>
<br>PHP will not &quot;take&quot; the changes until the webserver is restarted, and that doesn&apos;t mean through the MMC.&#xA0; Usually folks just reboot. But you can also use the following commands, for a much faster &quot;turnaround&quot;.&#xA0; At a command line prompt, type:
<br>
<br>iisreset /stop
<br>
<br>and that will stop the webserver service.&#xA0; Then type:
<br>
<br>net start w3svc
<br>
<br>and that will start the webserver service again.&#xA0; MUCH faster than a reboot, and you can check your changes faster as a result with the old:
<br>
<br>&lt;?php&gt;
<br>phpinfo();
<br>?&gt;
<br>
<br>in your page somewhere.
<br>
<br>I wish I could remember where I read this tip; it isn&apos;t anything I came up with...</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can have multiple versions of PHP running on the same Apache server. I have seen many different solutions pointing at achieving this, but most of them required installing additional instances of Apache, redirecting ports/hosts, etc., which was not satisfying for me.<br>Finally, I have come up with the simplest solution I&apos;ve seen so far, limited to reconfiguring Apache&apos;s httpd.conf.<br><br>My goal is to have PHP5 as the default scripting language for .php files in my DocumentRoot (which is in my case d:/htdocs), and PHP4 for specified DocumentRoot subdirectories.<br><br>Here it is (Apache&apos;s httpd.conf contents):<br><br>---------------------------<br># replace with your PHP4 directory<br>ScriptAlias /php4/ &quot;c:/usr/php4/&quot;<br># replace with your PHP5 directory<br>ScriptAlias /php5/ &quot;c:/usr/php5/&quot;<br><br>AddType application/x-httpd-php .php<br>Action application/x-httpd-php &quot;/php5/php-cgi.exe&quot;<br><br># populate this for every directory with PHP4 code<br>&lt;Directory &quot;d:/htdocs/some_subdir&quot;&gt;<br>&#xA0; &#xA0; Action application/x-httpd-php &quot;/php4/php.exe&quot;<br>&#xA0; &#xA0; # directory where your PHP4 php.ini file is located at<br>&#xA0; &#xA0; SetEnv PHPRC &quot;c:/usr/php4&quot;<br>&lt;/Directory&gt;<br><br># remember to put this section below the above<br>&lt;Directory &quot;d:/htdocs&quot;&gt;<br>&#xA0; &#xA0; # directory where your PHP5 php.ini file is located at<br>&#xA0; &#xA0; SetEnv PHPRC &quot;c:/usr/php5&quot;<br>&lt;/Directory&gt;<br>---------------------------<br><br>This solution is not limited to having only two parallel versions of PHP. You can play with httpd.conf contents to have as many PHP versions configured as you want.<br>You can also use multiple php.ini configuration files for the same PHP version (but for different DocumentRoot subfolders), which might be useful in some cases.<br><br>Remember to put your php.ini files in directories specified in lines &quot;SetEnv PHPRC...&quot;, and make sure that there&apos;s no php.ini files in other directories (such as c:\windows in Windows).<br><br>And finally, as you can see, I run PHP in CGI mode. This has its advantages and limitations. If you have to run PHP as Apache module, then... sorry - you have to use other solution (the best advice as always is: Google it!).<br><br>Hope this helps someone.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/install.windows.php)

**[To root](/)**