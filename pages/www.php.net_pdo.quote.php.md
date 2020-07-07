# PDO::quote





When converting from the old mysql_ functions to PDO, note that the quote function isn&apos;t exactly the same as the old mysql_real_escape_string function. It escapes, but also adds quotes; hence the name I guess :-)

After I replaced mysql_real_escape_string with $pdo-&gt;quote, it took me a bit to figure out why my strings were turning up in results with quotes around them. I felt like a fool when I realized all I needed to do was change ...\&quot;&quot;.$pdo-&gt;quote($foo).&quot;\&quot;... to ...&quot;.$pdo-&gt;quote($foo).&quot;...

  

#



PDO quote (tested with mysql and mariadb 10.3) is extremely slow.

It took me hours of debugging my performance issues until I found that pdo-&gt;quote is the problem.

This function is far from fast, and it&apos;s PHP instead of C code:
function escape($value)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $search = array(&quot;\\&quot;,&#xA0; &quot;\x00&quot;, &quot;\n&quot;,&#xA0; &quot;\r&quot;,&#xA0; &quot;&apos;&quot;,&#xA0; &apos;&quot;&apos;, &quot;\x1a&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; $replace = array(&quot;\\\\&quot;,&quot;\\0&quot;,&quot;\\n&quot;, &quot;\\r&quot;, &quot;\&apos;&quot;, &apos;\&quot;&apos;, &quot;\\Z&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; return str_replace($search, $replace, $value);
&#xA0; &#xA0; }

It is 50 times faster than pdo-&gt;quote()
(note, it&apos;s without quotes just escaping and only used here as an example)

  

#



One have to understand that string formatting has nothing to do with identifiers.
And thus string formatting should NEVER ever be used to format an identifier ( table of field name).
To quote an identifier, you have to format it as identifier, not as string.
To do so you have to

- Enclose identifier in backticks.
- Escape backticks inside by doubling them.

So, the code would be:


```
<?php
function quoteIdent($field) {
&#xA0; &#xA0; return &quot;`&quot;.str_replace(&quot;`&quot;,&quot;``&quot;,$field).&quot;`&quot;;
}
?>
```

this will make your identifier properly formatted and thus invulnerable to injection. 

However, there is another possible attack vector - using dynamical identifiers in the query may give an outsider control over fields the aren&apos;t allowed to:
Say, a field user_role in the users table and a dynamically built INSERT query based on a $_POST array may allow a privilege escalation with easily forged $_POST array. 
Or a select query which let a user to choose fields to display may reveal some sensitive information to attacker.

To prevent this kind of attack yet keep queries dynamic, one ought to use WHITELISTING approach.

Every dynamical identifier have to be checked against a hardcoded whitelist like this:


```
<?php
$allowed&#xA0; = array(&quot;name&quot;,&quot;price&quot;,&quot;qty&quot;);
$key = array_search($_GET[&apos;field&apos;], $allowed));
if ($key == false) {
&#xA0; &#xA0; throw new Exception(&apos;Wrong field name&apos;);
}
$field = $db-&gt;quoteIdent($allowed[$key]);
$query = &quot;SELECT $field FROM t&quot;; //value is safe
?>
```

(Personally I wouldn&apos;t use a query like this, but that&apos;s just an example of using a dynamical identifier in the query).

And similar approach have to be used when filtering dynamical arrays for insert and update:



```
<?php
function filterArray($input,$allowed)
{
&#xA0; &#xA0; foreach(array_keys($input) as $key )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if ( !in_array($key,$allowed) )
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; unset($input[$key]);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; return $input;
}
//used like this
$allowed = array(&apos;title&apos;,&apos;url&apos;,&apos;body&apos;,&apos;rating&apos;,&apos;term&apos;,&apos;type&apos;);
$data = $db-&gt;filterArray($_POST,$allowed); 
// $data now contains allowed fields only 
// and can be used to create INSERT or UPDATE query dynamically
?>
```



  

#



This function also converts new lines to \r\n

  

#



Note that this function just does what the documentation says: It escapes special characters in strings. 

It does NOT - however - detect a &quot;NULL&quot; value. If the value you try to quote is &quot;NULL&quot; it will return the same value as when you process an empty string (-&gt; &apos;&apos;), not the text &quot;NULL&quot;.

  

#

[Official documentation page](https://www.php.net/manual/en/pdo.quote.php)

**[To root](/README.md)**