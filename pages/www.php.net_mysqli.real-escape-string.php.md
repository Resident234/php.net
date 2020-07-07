# mysqli::real_escape_string





Note, that if no connection is open, mysqli_real_escape_string() will return an empty string!

  

#



For percent sign and underscore I use this:


```
<?php
$more_escaped = addcslashes($escaped, &apos;%_&apos;);
?>
```



  

#



Presenting several UTF-8 / Multibyte-aware escape functions.

These functions represent alternatives to mysqli::real_escape_string, as long as your DB connection and Multibyte extension are using the same character set (UTF-8), they will produce the same results by escaping the same characters as mysqli::real_escape_string.

This is based on research I did for my SQL Query Builder class:
https://github.com/twister-php/sql



```
<?php
/**
 * Returns a string with backslashes before characters that need to be escaped.
 * As required by MySQL and suitable for multi-byte character sets
 * Characters encoded are NUL (ASCII 0), \n, \r, \, &apos;, &quot;, and ctrl-Z.
 *
 * @param string $string String to add slashes to
 * @return $string with `\` prepended to reserved characters 
 *
 * @author Trevor Herselman
 */
if (function_exists(&apos;mb_ereg_replace&apos;))
{
&#xA0; &#xA0; function mb_escape(string $string)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return mb_ereg_replace(&apos;[\x00\x0A\x0D\x1A\x22\x27\x5C]&apos;, &apos;\\\0&apos;, $string);
&#xA0; &#xA0; }
} else {
&#xA0; &#xA0; function mb_escape(string $string)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return preg_replace(&apos;~[\x00\x0A\x0D\x1A\x22\x27\x5C]~u&apos;, &apos;\\\$0&apos;, $string);
&#xA0; &#xA0; }
}

?>
```


Characters escaped are (the same as mysqli::real_escape_string):

00 = \0 (NUL)
0A = \n
0D = \r
1A = ctl-Z
22 = &quot;
27 = &apos;
5C = \

Note: preg_replace() is in PCRE_UTF8 (UTF-8) mode (`u`).

Enhanced version:

When escaping strings for `LIKE` syntax, remember that you also need to escape the special characters _ and %

So this is a more fail-safe version (even when compared to mysqli::real_escape_string, because % characters in user input can cause unexpected results and even security violations via SQL injection in LIKE statements):



```
<?php

/**
 * Returns a string with backslashes before characters that need to be escaped.
 * As required by MySQL and suitable for multi-byte character sets
 * Characters encoded are NUL (ASCII 0), \n, \r, \, &apos;, &quot;, and ctrl-Z.
 * In addition, the special control characters % and _ are also escaped,
 * suitable for all statements, but especially suitable for `LIKE`.
 *
 * @param string $string String to add slashes to
 * @return $string with `\` prepended to reserved characters 
 *
 * @author Trevor Herselman
 */
if (function_exists(&apos;mb_ereg_replace&apos;))
{
&#xA0; &#xA0; function mb_escape(string $string)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return mb_ereg_replace(&apos;[\x00\x0A\x0D\x1A\x22\x25\x27\x5C\x5F]&apos;, &apos;\\\0&apos;, $string);
&#xA0; &#xA0; }
} else {
&#xA0; &#xA0; function mb_escape(string $string)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return preg_replace(&apos;~[\x00\x0A\x0D\x1A\x22\x25\x27\x5C\x5F]~u&apos;, &apos;\\\$0&apos;, $string);
&#xA0; &#xA0; }
}

?>
```


Additional characters escaped:

25 = %
5F = _

Bonus function:

The original MySQL `utf8` character-set (for tables and fields) only supports 3-byte sequences.
4-byte characters are not common, but I&apos;ve had queries fail to execute on 4-byte UTF-8 characters, so you should be using `utf8mb4` wherever possible.

However, if you still want to use `utf8`, you can use the following function to replace all 4-byte sequences.



```
<?php
// Modified from: https://stackoverflow.com/a/24672780/2726557
function mysql_utf8_sanitizer(string $str)
{
&#xA0; &#xA0; return preg_replace(&apos;/[\x{10000}-\x{10FFFF}]/u&apos;, &quot;\xEF\xBF\xBD&quot;, $str);
}
?>
```


Pick your poison and use at your own risk!

  

#



You can avoid all character escaping issues (on the PHP side) if you use prepare() and bind_param(), as an alternative to placing arbitrary string values in SQL statements.&#xA0; This works because bound parameter values are NOT passed via the SQL statement syntax.

  

#



To escape for the purposes of having your queries made successfully, and to prevent SQLi (SQL injection)/stored and/or reflected XSS, it&apos;s a good idea to go with the basics first, then make sure nothing gets in that can be used for SQLi or stored/reflected XSS, or even worse, loading remote images and scripts.

For example:



```
<?php
&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0;&#xA0; // Assume this is a simple comments form with a name and comment.

&#xA0; &#xA0;&#xA0; $name = mysqli_real_escape_string($conn, $_POST[&apos;name&apos;]);
&#xA0; &#xA0;&#xA0; $comments = mysqli_real_escape_string($conn, $_POST[&apos;comments&apos;]);

&#xA0; &#xA0;&#xA0; // Here is where most of the action happens.&#xA0; But see note below
&#xA0; &#xA0;&#xA0; // on dumping back out from the database

&#xA0; &#xA0;&#xA0; // We should use the ENT_QUOTES flag second parameter...
&#xA0; &#xA0;&#xA0; $name = htmlspecialchars($name);
&#xA0; &#xA0;&#xA0; $comments = htmlspecialchars($comments);

&#xA0; &#xA0;&#xA0; $insert_sql = &quot;INSERT INTO tbl_comments ( c_id, c_name, c_comments ) VALUES ( DEFAULT, &apos;&quot; . $name . &quot;&apos;, &apos;&quot; . $comments . &quot;&apos;)&quot;;

&#xA0; &#xA0;&#xA0; $res = mysqli_query($conn, $insert_sql);
&#xA0; &#xA0;&#xA0; if ( $res === false ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Something went wrong, handle it
&#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; // Now output page showing comments
?>
```


//&#xA0; Assume we&apos;re in a table with each row containing a name and comment



```
<?php
&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0;&#xA0; $res = mysqli_query($conn, &quot;SELECT c_name, c_comments FROM tbl_comments ORDER BY c_name ASC&quot;);

&#xA0; &#xA0;&#xA0; if ( $res === false )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Something went wrong

&#xA0; &#xA0;&#xA0; // Or as you like...
&#xA0; &#xA0;&#xA0; while ( $row = mysqli_fetch_array($res, MYSQLI_BOTH) ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // This will output safe HTML entities if they went in
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // They will be displayed, but not interpreted
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;tr&gt;&lt;td&gt;&quot; . $row[&apos;c_name&apos;] . &quot;&lt;/td&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;td&gt;&quot; . $row[&apos;c_comments&apos;] . &quot;&lt;/td&gt;&lt;/tr&gt;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // BUT, if you make this mistake...
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;tr&gt;&lt;td&gt;&quot; . htmlspecialchars_decode($row[&apos;c_name&apos;]) . &quot;&lt;/td&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;td&gt;&quot; . htmlspecialchars_decode($row[&apos;c_comments&apos;]) . &quot;&lt;/td&gt;&lt;/tr&gt;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ... then your entities will reflect back as the characters, so
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // input such as this: &quot;&gt;&lt;img src=x onerror=alert(&apos;xss&apos;)&gt;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // will display the &apos;xss&apos; in an alert box in the browser.
&#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; mysqli_free_result($res);
&#xA0; &#xA0;&#xA0; mysqli_close($conn);
?>
```


In most cases, you wouldn&apos;t want to go way overboard sanitizing untrusted user input, for instance:



```
<?php
&#xA0; &#xA0;&#xA0; $my_input = htmlspecialchars( strip_tags($_POST[&apos;foo&apos;]) );
?>
```


This will junk a lot of input you might actually want, if you&apos;re rolling your own forum or comments section and it&apos;s for web developers, for example.&#xA0; On the other hand, if legitimate users are never going to enter anything other than text, never HTML tags or anything else, it&apos;s not a bad idea.

The take-away is that mysqli_real_escape_string() is not good enough, and being overly-aggressive in sanitizing input may not be what you want.

Be aware that in the above example, it will protect you from sqli (run sqlmap on all your input fields and forms to check) but it won&apos;t protect your database from being filled with junk, effectively DoS&apos;ing your Web app in the process.

So after protecting against SQLi, even if you&apos;re behind CloudFlare and take other measures to protect your databases, there&apos;s still effectively a DoS attack that could slow down your Web App for legitimate users and make it a nightmare filled with rubbish that some poor maintainer has to clean out, if you don&apos;t take other measures.

So aside from escaping your stings, and protecting against SQLi and stored/reflected XSS, and maliciously loaded images or JS, there&apos;s also checking your input to see if it makes sense, so you don&apos;t get a database full of rubbish!

It just never ends... :-)

  

#



Note that this function will NOT escape _ (underscore) and % (percent) signs, which have special meanings in LIKE clauses. 

As far as I know there is no function to do this, so you have to escape them yourself by adding a backslash in front of them.

  

#



When I submit data through Ajax I use a little function to reconvert the encoded chars to their original value. After that I do the escaping. Here the function:

&#xA0;&#xA0; function my_htmlentities($input){
&#xA0; &#xA0; &#xA0;&#xA0; $string = htmlentities($input,ENT_NOQUOTES,&apos;UTF-8&apos;);
&#xA0; &#xA0; &#xA0;&#xA0; $string = str_replace(&apos;&amp;euro;&apos;,chr(128),$string);
&#xA0; &#xA0; &#xA0;&#xA0; $string = html_entity_decode($string,ENT_NOQUOTES,&apos;ISO-8859-15&apos;);
&#xA0; &#xA0; &#xA0;&#xA0; return $string;
&#xA0;&#xA0; }

G.Zanferrari

  

#



If you wonder why (besides \, &apos; and &quot;)&#xA0; NUL (ASCII 0), \n, \r, and Control-Z are escaped: it is not to prevent sql injection, but to prevent your sql logfile to get unreadable.

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.real-escape-string.php)

**[To root](/README.md)**