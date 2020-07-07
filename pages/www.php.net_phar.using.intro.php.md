# Using Phar Archives: Introduction





If you are trying to use Phar for a web application and just getting a blank screen, if you have enabled suhosin as well you have to add:

suhosin.executor.include.whitelist=&quot;phar&quot;

to &quot;/etc/php5/conf.d/suhosin.ini&quot; file or your &quot;php.ini&quot; file.

once done everything works fine and dandy.

  

#

[Official documentation page](https://www.php.net/manual/en/phar.using.intro.php)

**[To root](/README.md)**