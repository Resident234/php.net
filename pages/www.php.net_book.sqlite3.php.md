# SQLite3



As of PHP 5.4 support for Sqlite2 has been removed. I have a large web app that was built with sqlite2 as the database backend and thus it exploded when I updated PHP. If you&apos;re in a similar situation I&apos;ve written a few wrapper functions that will allow your app to work whilst you convert the code to sqlite3. <br><br>Firstly convert your DB to an sqlite3 db. <br><br>sqlite OLD.DB .dump | sqlite3 NEW.DB<br><br>Then add the following functions to your app:<br><br>

```
<?php
function sqlite_open($location,$mode)
{
    $handle = new SQLite3($location);
    return $handle;
}
function sqlite_query($dbhandle,$query)
{
    $array['dbhandle'] = $dbhandle;
    $array['query'] = $query;
    $result = $dbhandle->query($query);
    return $result;
}
function sqlite_fetch_array(&amp;$result,$type)
{
    #Get Columns
    $i = 0;
    while ($result->columnName($i))
    {
        $columns[ ] = $result->columnName($i);
        $i++;
    }
    
    $resx = $result->fetchArray(SQLITE3_ASSOC);
    return $resx;
}
?>
```
<br><br>They&apos;re not perfect by any stretch but they seem to be working ok as a temporary measure while I convert the site. <br>Hope that helps someone  

#

[Official documentation page](https://www.php.net/manual/en/book.sqlite3.php)

**[To root](/README.md)**