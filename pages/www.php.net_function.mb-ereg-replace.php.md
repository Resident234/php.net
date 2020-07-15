# mb_ereg_replace



Unlike preg_replace, mb_ereg_replace doesn&apos;t use separators<br><br>Exemple with preg_replace :<br>

```
<?php $data = preg_replace("/[^A-Za-z0-9\.\-]/","",$data); ?>
```


Exemple with mb_ereg_replace :


```
<?php $data = mb_ereg_replace("[^A-Za-z0-9\.\-]","",$data); ?>
```
  

#

I got a pretty nasty error while trying to parse table rows(all contents were set to UTF-8) from the database for a dictionary project. The idea was to get all the rows from the first table (that is a table with bulgarian phrase in the first field, and its translation in english, french and german in the next fields). I needed to index all the bulgarian words that are found in the table to make an intelligent search. And that is where my headache started.<br><br>First of all, even with mb_strtolower() a lot of cyrillic characters went corrupted (ex: &apos;&#x442;,&#x44A;,&#x443;,&#x444;,&#x431;,&#x433;,&#x437;,&#x436;,&apos; etc...). After an hour of different attempts I got such a solution:<br><br>

```
<?php

mb_internal_encoding("UTF-8");
mb_regex_encoding("UTF-8");

$rows = $db->getRows();

$contents = array();
foreach ($rows as $eachRow)
{
    $cleared = str_replace($commonWords, ' ', mb_strtolower(stripslashes($eachRow['bulgarian']), 'UTF-8' ));
    if (trim($cleared) != '') $contents[] = trim($cleared);
}    

$list = array();
foreach ($contents as $eachRow)
{
    $exploded = explode(' ', $eachRow);
    foreach ($exploded as $eachExpl)
    {
        $eachExpl = mb_ereg_replace('[^&#x430;-&#x44F; ]',' ', $eachExpl);
        if (trim($eachExpl) != '') 
            if (!in_array($eachExpl, $list, true))    $list[] = trim($eachExpl);
    }
}

?>
```
<br><br>To work properly I got to set all the internal encoding settings to UTF-8. Else the default Latin-1 got half my database with missing characters.<br><br>I am posting this solution just in case someone has encountered a similar problem. Hope it helps you in case you need something like that.  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-ereg-replace.php)

**[To root](/README.md)**