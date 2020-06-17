# Using Phar Archives: Introduction




<div class="phpcode"><span class="html">
If you are trying to use Phar for a web application and just getting a blank screen, if you have enabled suhosin as well you have to add:<br><br>suhosin.executor.include.whitelist=&quot;phar&quot;<br><br>to &quot;/etc/php5/conf.d/suhosin.ini&quot; file or your &quot;php.ini&quot; file.<br><br>once done everything works fine and dandy.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/phar.using.intro.php)

**[To root](/README.md)**