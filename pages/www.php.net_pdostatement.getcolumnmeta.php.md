# PDOStatement::getColumnMeta



This method is supported in the MySQL 5.0+ driver.  It can be used for object hydration:<br><br>

```
<?php

$pdo_stmt = $dbh-&gt;execute(&apos;SELECT discussion.id, discussion.text, comment.id, comment.text FROM discussions LEFT JOIN comments ON comment.discussion_id = discussion.id&apos;);

foreach(range(0, $pdo_stmt-&gt;columnCount() - 1) as $column_index)
{
  $meta[] = $pdo_stmt-&gt;getColumnMeta($column_index);
}

while($row = $pdo_stmt-&gt;fetch(PDO::FETCH_NUM))
{
  foreach($row as $column_index =&gt; $column_value)
  {
    //do something with the data, using the ids to establish the discussion.has_many(comments) relationship.
  }
}

?>
```
<br><br>If you are building an ORM, this method is very useful to support more natural SQL syntax.  Most ORMs require the column names to be aliases so that they can be parsed and turned into objects that properly represent has_one, has_many, many_to_many relationships.  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.getcolumnmeta.php)

**[To root](/README.md)**