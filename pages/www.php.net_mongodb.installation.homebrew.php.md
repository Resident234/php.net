# Installing the MongoDB PHP Driver on macOS with Homebrew




<div class="phpcode"><span class="html">
How to install on macOS Mojave<br><br>Start with:<br><br>sudo pecl install mongodb<br><br>To check if mongodb package is installed, look for &quot;mongodb&quot; when you run:<br><br>pecl list<br><br>To get your installed mongodb.so path, run:<br><br>pecl list-files mongodb | grep mongodb.so<br><br>then remove (or comment out) on your php.ini file:<br><br>extension=&quot;mongodb.so&quot; <br><br>(I could not found a line with extension=&quot;php_mongodb.so&quot;)<br><br>now insert a line with:<br><br>extension=&quot;{{the path to your installed mongodb.so}}&quot;<br><br>Run this command to get your ext-*.ini directory path:<br><br>php -i | grep Scan<br><br>Create your ext-mongodb.ini with:<br><br>touch {{your conf.d path}}/ext-mongodb.ini<br><br>like: <br>touch /usr/local/etc/php/7.1/conf.d/ext-mongodb.ini<br><br>Restart your apache to read the new configuration<br><br>Sanity check with:<br><br>php -i | grep mongodb <br><br>Your&apos;re ready to go.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The mongodb install has been removed from Homebrew. To install the mongodb extension you need to use pecl.<br><br>sudo pecl install mongodb<br><br>You may need to follow some additional configuration. Follow the inline instructions as they appear. Once the install is finished it will add two lines at the bottom of your php.ini file. Instead, remove these lines and add the and add a separate file to your conf.d directory in the same directory as you php.ini file.<br><br>In php.ini remove<br><br>extension=&quot;mongodb.so&quot; // remove<br>extension=&quot;php_mongodb.so // remove <br><br>Then run:<br><br>$ touch /usr/local/etc/php/5.6/conf.d/ext-mongodb.ini<br><br>Finally add this line to the new file:<br><br>// example<br>extension=&quot;/usr/local/Cellar/php@5.6/5.6.35/pecl/20131226/mongodb.so&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.installation.homebrew.php)

**[To root](/README.md)**