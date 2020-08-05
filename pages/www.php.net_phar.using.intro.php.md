# Using Phar Archives: Introduction



If you are trying to use Phar for a web application and just getting a blank screen, if you have enabled suhosin as well you have to add:<br><br>suhosin.executor.include.whitelist="phar"<br><br>to "/etc/php5/conf.d/suhosin.ini" file or your "php.ini" file.<br><br>once done everything works fine and dandy.  

---

[Official documentation page](https://www.php.net/manual/en/phar.using.intro.php)

**[To root](/README.md)**