# PHP Data Objects




<div class="phpcode"><span class="html">
Please note this:<br><br>Won&apos;t work:<br>$sth = $dbh-&gt;prepare(&apos;SELECT name, colour, calories FROM ?&#xA0; WHERE calories &lt; ?&apos;);<br><br>THIS WORKS!<br>$sth = $dbh-&gt;prepare(&apos;SELECT name, colour, calories FROM fruit WHERE calories &lt; ?&apos;);<br><br>The parameter cannot be applied on table names!!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.pdo.php)

**[To root](/README.md)**