# PHP Data Objects





Please note this:

Won&apos;t work:
$sth = $dbh-&gt;prepare(&apos;SELECT name, colour, calories FROM ?&#xA0; WHERE calories &lt; ?&apos;);

THIS WORKS!
$sth = $dbh-&gt;prepare(&apos;SELECT name, colour, calories FROM fruit WHERE calories &lt; ?&apos;);

The parameter cannot be applied on table names!!

  

#

[Official documentation page](https://www.php.net/manual/en/book.pdo.php)

**[To root](/README.md)**