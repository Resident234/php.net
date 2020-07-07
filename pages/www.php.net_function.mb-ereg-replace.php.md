# mb_ereg_replace





Unlike preg_replace, mb_ereg_replace doesn&apos;t use separators



Exemple with preg_replace :



```
<?php $data = preg_replace(&quot;/[^A-Za-z0-9\.\-]/&quot;,&quot;&quot;,$data); ?>
```




Exemple with mb_ereg_replace :



```
<?php $data = mb_ereg_replace(&quot;[^A-Za-z0-9\.\-]&quot;,&quot;&quot;,$data); ?>
```



  

#



I got a pretty nasty error while trying to parse table rows(all contents were set to UTF-8) from the database for a dictionary project. The idea was to get all the rows from the first table (that is a table with bulgarian phrase in the first field, and its translation in english, french and german in the next fields). I needed to index all the bulgarian words that are found in the table to make an intelligent search. And that is where my headache started.

First of all, even with mb_strtolower() a lot of cyrillic characters went corrupted (ex: &apos;&#x442;,&#x44A;,&#x443;,&#x444;,&#x431;,&#x433;,&#x437;,&#x436;,&apos; etc...). After an hour of different attempts I got such a solution:



```
<?php

mb_internal_encoding(&quot;UTF-8&quot;);
mb_regex_encoding(&quot;UTF-8&quot;);

$rows = $db-&gt;getRows();

$contents = array();
foreach ($rows as $eachRow)
{
&#xA0; &#xA0; $cleared = str_replace($commonWords, &apos; &apos;, mb_strtolower(stripslashes($eachRow[&apos;bulgarian&apos;]), &apos;UTF-8&apos; ));
&#xA0; &#xA0; if (trim($cleared) != &apos;&apos;) $contents[] = trim($cleared);
}&#xA0; &#xA0; 

$list = array();
foreach ($contents as $eachRow)
{
&#xA0; &#xA0; $exploded = explode(&apos; &apos;, $eachRow);
&#xA0; &#xA0; foreach ($exploded as $eachExpl)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $eachExpl = mb_ereg_replace(&apos;[^&#x430;-&#x44F; ]&apos;,&apos; &apos;, $eachExpl);
&#xA0; &#xA0; &#xA0; &#xA0; if (trim($eachExpl) != &apos;&apos;) 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!in_array($eachExpl, $list, true))&#xA0; &#xA0; $list[] = trim($eachExpl);
&#xA0; &#xA0; }
}

?>
```


To work properly I got to set all the internal encoding settings to UTF-8. Else the default Latin-1 got half my database with missing characters.

I am posting this solution just in case someone has encountered a similar problem. Hope it helps you in case you need something like that.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-ereg-replace.php)

**[To root](/README.md)**