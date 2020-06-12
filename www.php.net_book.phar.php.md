# Phar




<div class="phpcode"><span class="html">
Here is an apache2 htaccess example that prevents the downloading of phar-Archives by the user:<br><br>RewriteEngine on<br>RewriteRule ^(.*)\.phar$ - [F]<br><br>It triggers a &quot;403 - Forbidden&quot; message instead of delivering the archive.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you get blank pages when trying to access a phar web-page, then you probably have Suhosin on your PHP (like in Debian and Ubuntu), and you need to ad this to your php.ini to allow execution of PHAR archives :<br><br>suhosin.executor.include.whitelist=&quot;phar&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.phar.php)

**[⬆ to root](/)**