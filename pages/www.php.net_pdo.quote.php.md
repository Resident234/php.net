# PDO::quote



When converting from the old mysql_ functions to PDO, note that the quote function isn&apos;t exactly the same as the old mysql_real_escape_string function. It escapes, but also adds quotes; hence the name I guess :-)<br><br>After I replaced mysql_real_escape_string with $pdo-&gt;quote, it took me a bit to figure out why my strings were turning up in results with quotes around them. I felt like a fool when I realized all I needed to do was change ...\"".$pdo-&gt;quote($foo)."\"... to ...".$pdo-&gt;quote($foo)."...  

#

PDO quote (tested with mysql and mariadb 10.3) is extremely slow.<br><br>It took me hours of debugging my performance issues until I found that pdo-&gt;quote is the problem.<br><br>This function is far from fast, and it&apos;s PHP instead of C code:<br>function escape($value)<br>    {<br>        $search = array("\\",  "\x00", "\n",  "\r",  "&apos;",  &apos;"&apos;, "\x1a");<br>        $replace = array("\\\\","\\0","\\n", "\\r", "\&apos;", &apos;\"&apos;, "\\Z");<br><br>        return str_replace($search, $replace, $value);<br>    }<br><br>It is 50 times faster than pdo-&gt;quote()<br>(note, it&apos;s without quotes just escaping and only used here as an example)  

#

One have to understand that string formatting has nothing to do with identifiers.<br>And thus string formatting should NEVER ever be used to format an identifier ( table of field name).<br>To quote an identifier, you have to format it as identifier, not as string.<br>To do so you have to<br><br>- Enclose identifier in backticks.<br>- Escape backticks inside by doubling them.<br><br>So, the code would be:<br>

```
<?php
function quoteIdent($field) {
    return "`".str_replace("`","``",$field)."`";
}
?>
```

this will make your identifier properly formatted and thus invulnerable to injection. 

However, there is another possible attack vector - using dynamical identifiers in the query may give an outsider control over fields the aren't allowed to:
Say, a field user_role in the users table and a dynamically built INSERT query based on a $_POST array may allow a privilege escalation with easily forged $_POST array. 
Or a select query which let a user to choose fields to display may reveal some sensitive information to attacker.

To prevent this kind of attack yet keep queries dynamic, one ought to use WHITELISTING approach.

Every dynamical identifier have to be checked against a hardcoded whitelist like this:


```
<?php
$allowed  = array("name","price","qty");
$key = array_search($_GET['field'], $allowed));
if ($key == false) {
    throw new Exception('Wrong field name');
}
$field = $db->quoteIdent($allowed[$key]);
$query = "SELECT $field FROM t"; //value is safe
?>
```

(Personally I wouldn't use a query like this, but that's just an example of using a dynamical identifier in the query).

And similar approach have to be used when filtering dynamical arrays for insert and update:



```
<?php
function filterArray($input,$allowed)
{
    foreach(array_keys($input) as $key )
    {
        if ( !in_array($key,$allowed) )
        {
             unset($input[$key]);
        }
    }
    return $input;
}
//used like this
$allowed = array('title','url','body','rating','term','type');
$data = $db->filterArray($_POST,$allowed); 
// $data now contains allowed fields only 
// and can be used to create INSERT or UPDATE query dynamically
?>
```
  

#

This function also converts new lines to \r\n  

#

Note that this function just does what the documentation says: It escapes special characters in strings. <br><br>It does NOT - however - detect a "NULL" value. If the value you try to quote is "NULL" it will return the same value as when you process an empty string (-&gt; &apos;&apos;), not the text "NULL".  

#

[Official documentation page](https://www.php.net/manual/en/pdo.quote.php)

**[To root](/README.md)**