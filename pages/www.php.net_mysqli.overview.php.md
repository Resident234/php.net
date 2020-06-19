# Overview




<div class="phpcode"><span class="html">
The text: &quot;PDO does not allow you to use MySQL&apos;s support for Multiple Statements&quot; is outdated.<br><br>Since v5.3, PHP intoduced multiple statement support into PDO (by PDO_MYSQLND driver replacing the previous PDO_MYSQL).</span>
</div>
  

#


<div class="phpcode"><span class="html">
mysqli can be great in some circumstances but much work has been put into PHP Portable Data Objects (PDO) which you should also consider when choosing a way to connect to your database using php. For example, PDO supports MySQL with minimal performance hit and the code your write for it will support many other databases with little or no changes. That said, the database connection code, even if you have to change a lot of it to use another database will be much less work than coding your actual database data entry and report apps. When I started creating PHP/MySQL apps years ago, I used php&apos;s native support for PHP then moved to PEAR:DB and MDB/MDB2 and finally to wizzyweb which is basically like &quot;phpMyAdmin for Apps&quot; to create apps as it automatically generates the PHP PDO connection code and the application code. Sure, I could code it all from scratch but I save about 90% of the time it used to take. The point is look at the total amount of time you will save by using native code vs. an abstraction layer. Most people find that programmer time is the most valuable part of the equation so anything than can save programmer time should be heavily weighted.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.overview.php)

**[To root](/README.md)**