# PDO::prepare



To those wondering why adding quotes to around a placeholder is wrong, and why you can&apos;t use placeholders for table or column names:<br><br>There is a common misconception about how the placeholders in prepared statements work: they are not simply substituted in as (escaped) strings, and the resulting SQL executed. Instead, a DBMS asked to "prepare" a statement comes up with a complete query plan for how it would execute that query, including which tables and indexes it would use, which will be the same regardless of how you fill in the placeholders.<br><br>The plan for "SELECT name FROM my_table WHERE id = :value" will be the same whatever you substitute for ":value", but the seemingly similar "SELECT name FROM :table WHERE id = :value" cannot be planned, because the DBMS has no idea what table you&apos;re actually going to select from.<br><br>Even when using "emulated prepares", PDO cannot let you use placeholders anywhere, because it would have to work out what you meant: does "Select :foo From some_table" mean ":foo" is going to be a column reference, or a literal string?<br><br>When your query is using a dynamic column reference, you should be explicitly white-listing the columns you know to exist on the table, e.g. using a switch statement with an exception thrown in the default: clause.  

---

Hi All,<br><br>First time posting to php.net, a little nervous.<br><br>After a bunch of searching I&apos;ve learned 2 things about prepared statements:<br>1.) It fails if you enclose in a single quote (&apos;)<br>This fails: "SELECT * FROM users WHERE email=&apos;:email&apos;"<br>This works: "SELECT * FROM users WHERE email=:email"<br>2.) You cannot search with a prepared statement<br>This fails: "SELECT * FROM users WHERE :search=:email"<br>This succeeds: "SELECT * FROM users WHERE $search=:email"<br><br>In my case I allow the user to enter their username or email, determine which they&apos;ve entered and set $search to "username" or "email". As this value is not entered by the user there is no potential for SQL injection and thus safe to use as I have done.<br><br>Hope that saves someone else from a lot of searching.  

---

You can also pass an array of values to PDOStatement::execute(). This is also secured against SQL injection. You don&apos;t necessarily have to use bindParam() or bindValue().  

---

if you run queries in a loop, don&apos;t include $pdo-&gt;prepare() inside the loop, it will save you some resources (and time).<br><br>prepare statement inside loop:<br>for($i=0; $i&lt;1000; $i++) {<br>    $rs = $pdo-&gt;prepare("SELECT `id` FROM `admins` WHERE `groupID` = :groupID AND `id` &lt;&gt; :id");<br>    $rs-&gt;execute([&apos;:groupID&apos; =&gt; $group, &apos;:id&apos; =&gt; $id]);<br>}<br><br>// took 0.066626071929932 microseconds<br><br>prepare statement outside loop:<br>$rs = $pdo-&gt;prepare("SELECT `id` FROM `admins` WHERE `groupID` = :groupID AND `id` &lt;&gt; :id");<br>for($i=0; $i&lt;1000; $i++) {<br>    $rs-&gt;execute([&apos;:groupID&apos; =&gt; $group, &apos;:id&apos; =&gt; $id]);<br>}<br><br>// took 0.064448118209839 microseconds<br><br>for 1,000 (simple) queries it took 0.002 microseconds less.<br>not much, but it worth mention.  

---

Note on the SQL injection properties of prepared statements.<br><br>Prepared statements only project you from SQL injection IF you use the bindParam or bindValue option.<br><br>For example if you have a table called users with two fields, username and email and someone updates their username you might run<br><br>UPDATE `users` SET `user`=&apos;$var&apos;<br><br>where $var would be the user submitted text. <br><br>Now if you did <br>

```
<?php
$a=new PDO("mysql:host=localhost;dbname=database;","root","");
$b=$a->prepare("UPDATE `users` SET user='$var'");
$b->execute();
?>
```


and the user had entered  User', email='test for a test the injection would occur and the email would be updated to test as well as the user being updated to User.

Using bindParam as follows
 

```
<?php
$var="User', email='test";
$a=new PDO("mysql:host=localhost;dbname=database;","root","");
$b=$a->prepare("UPDATE `users` SET user=:var");
$b->bindParam(":var",$var);
$b->execute();
?>
```
<br><br>The sql would be escaped and update the username to User&apos;, email=&apos;test&apos;  

---

With PDO_MYSQL you need to remember about the PDO::ATTR_EMULATE_PREPARES option.<br><br>The default value is TRUE, like<br>$dbh-&gt;setAttribute(PDO::ATTR_EMULATE_PREPARES,true); <br><br>This means that no prepared statement is created with $dbh-&gt;prepare() call. With exec() call PDO replaces the placeholders with values itself and sends MySQL a generic query string.<br><br>The first consequence is that the call  $dbh-&gt;prepare(&apos;garbage&apos;);<br>reports no error. You will get an SQL error during the $dbh-&gt;exec() call.<br>The second one is the SQL injection risk in special cases, like using a placeholder for the table name.<br><br>The reason for emulation is a poor performance of MySQL with prepared statements. Emulation works significantly faster.  

---

[Official documentation page](https://www.php.net/manual/en/pdo.prepare.php)

**[To root](/README.md)**