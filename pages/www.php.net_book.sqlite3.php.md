# SQLite3





As of PHP 5.4 support for Sqlite2 has been removed. I have a large web app that was built with sqlite2 as the database backend and thus it exploded when I updated PHP. If you&apos;re in a similar situation I&apos;ve written a few wrapper functions that will allow your app to work whilst you convert the code to sqlite3. 



Firstly convert your DB to an sqlite3 db. 



sqlite OLD.DB .dump | sqlite3 NEW.DB



Then add the following functions to your app:





```
<?php

function sqlite_open($location,$mode)

{

&#xA0; &#xA0; $handle = new SQLite3($location);

&#xA0; &#xA0; return $handle;

}

function sqlite_query($dbhandle,$query)

{

&#xA0; &#xA0; $array[&apos;dbhandle&apos;] = $dbhandle;

&#xA0; &#xA0; $array[&apos;query&apos;] = $query;

&#xA0; &#xA0; $result = $dbhandle-&gt;query($query);

&#xA0; &#xA0; return $result;

}

function sqlite_fetch_array(&amp;$result,$type)

{

&#xA0; &#xA0; #Get Columns

&#xA0; &#xA0; $i = 0;

&#xA0; &#xA0; while ($result-&gt;columnName($i))

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $columns[ ] = $result-&gt;columnName($i);

&#xA0; &#xA0; &#xA0; &#xA0; $i++;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; $resx = $result-&gt;fetchArray(SQLITE3_ASSOC);

&#xA0; &#xA0; return $resx;

}

?>
```




They&apos;re not perfect by any stretch but they seem to be working ok as a temporary measure while I convert the site. 

Hope that helps someone

  

#

[Official documentation page](https://www.php.net/manual/en/book.sqlite3.php)

**[To root](/README.md)**