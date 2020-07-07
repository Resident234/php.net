# PDO::prepare





To those wondering why adding quotes to around a placeholder is wrong, and why you can&apos;t use placeholders for table or column names:

There is a common misconception about how the placeholders in prepared statements work: they are not simply substituted in as (escaped) strings, and the resulting SQL executed. Instead, a DBMS asked to &quot;prepare&quot; a statement comes up with a complete query plan for how it would execute that query, including which tables and indexes it would use, which will be the same regardless of how you fill in the placeholders.

The plan for &quot;SELECT name FROM my_table WHERE id = :value&quot; will be the same whatever you substitute for &quot;:value&quot;, but the seemingly similar &quot;SELECT name FROM :table WHERE id = :value&quot; cannot be planned, because the DBMS has no idea what table you&apos;re actually going to select from.

Even when using &quot;emulated prepares&quot;, PDO cannot let you use placeholders anywhere, because it would have to work out what you meant: does &quot;Select :foo From some_table&quot; mean &quot;:foo&quot; is going to be a column reference, or a literal string?

When your query is using a dynamic column reference, you should be explicitly white-listing the columns you know to exist on the table, e.g. using a switch statement with an exception thrown in the default: clause.

  

#



Hi All,

First time posting to php.net, a little nervous.

After a bunch of searching I&apos;ve learned 2 things about prepared statements:
1.) It fails if you enclose in a single quote (&apos;)
This fails: &quot;SELECT * FROM users WHERE email=&apos;:email&apos;&quot;
This works: &quot;SELECT * FROM users WHERE email=:email&quot;
2.) You cannot search with a prepared statement
This fails: &quot;SELECT * FROM users WHERE :search=:email&quot;
This succeeds: &quot;SELECT * FROM users WHERE $search=:email&quot;

In my case I allow the user to enter their username or email, determine which they&apos;ve entered and set $search to &quot;username&quot; or &quot;email&quot;. As this value is not entered by the user there is no potential for SQL injection and thus safe to use as I have done.

Hope that saves someone else from a lot of searching.

  

#



You can also pass an array of values to PDOStatement::execute(). This is also secured against SQL injection. You don&apos;t necessarily have to use bindParam() or bindValue().

  

#



Note on the SQL injection properties of prepared statements.

Prepared statements only project you from SQL injection IF you use the bindParam or bindValue option.

For example if you have a table called users with two fields, username and email and someone updates their username you might run

UPDATE `users` SET `user`=&apos;$var&apos;

where $var would be the user submitted text. 

Now if you did 


```
<?php
$a=new PDO(&quot;mysql:host=localhost;dbname=database;&quot;,&quot;root&quot;,&quot;&quot;);
$b=$a-&gt;prepare(&quot;UPDATE `users` SET user=&apos;$var&apos;&quot;);
$b-&gt;execute();
?>
```


and the user had entered&#xA0; User&apos;, email=&apos;test for a test the injection would occur and the email would be updated to test as well as the user being updated to User.

Using bindParam as follows
 

```
<?php
$var=&quot;User&apos;, email=&apos;test&quot;;
$a=new PDO(&quot;mysql:host=localhost;dbname=database;&quot;,&quot;root&quot;,&quot;&quot;);
$b=$a-&gt;prepare(&quot;UPDATE `users` SET user=:var&quot;);
$b-&gt;bindParam(&quot;:var&quot;,$var);
$b-&gt;execute();
?>
```


The sql would be escaped and update the username to User&apos;, email=&apos;test&apos;

  

#



if you run queries in a loop, don&apos;t include $pdo-&gt;prepare() inside the loop, it will save you some resources (and time).

prepare statement inside loop:
for($i=0; $i&lt;1000; $i++) {
&#xA0; &#xA0; $rs = $pdo-&gt;prepare(&quot;SELECT `id` FROM `admins` WHERE `groupID` = :groupID AND `id` &lt;&gt; :id&quot;);
&#xA0; &#xA0; $rs-&gt;execute([&apos;:groupID&apos; =&gt; $group, &apos;:id&apos; =&gt; $id]);
}

// took 0.066626071929932 microseconds

prepare statement outside loop:
$rs = $pdo-&gt;prepare(&quot;SELECT `id` FROM `admins` WHERE `groupID` = :groupID AND `id` &lt;&gt; :id&quot;);
for($i=0; $i&lt;1000; $i++) {
&#xA0; &#xA0; $rs-&gt;execute([&apos;:groupID&apos; =&gt; $group, &apos;:id&apos; =&gt; $id]);
}

// took 0.064448118209839 microseconds

for 1,000 (simple) queries it took 0.002 microseconds less.
not much, but it worth mention.

  

#



With PDO_MYSQL you need to remember about the PDO::ATTR_EMULATE_PREPARES option.

The default value is TRUE, like
$dbh-&gt;setAttribute(PDO::ATTR_EMULATE_PREPARES,true); 

This means that no prepared statement is created with $dbh-&gt;prepare() call. With exec() call PDO replaces the placeholders with values itself and sends MySQL a generic query string.

The first consequence is that the call&#xA0; $dbh-&gt;prepare(&apos;garbage&apos;);
reports no error. You will get an SQL error during the $dbh-&gt;exec() call.
The second one is the SQL injection risk in special cases, like using a placeholder for the table name.

The reason for emulation is a poor performance of MySQL with prepared statements. Emulation works significantly faster.

  

#

[Official documentation page](https://www.php.net/manual/en/pdo.prepare.php)

**[To root](/README.md)**