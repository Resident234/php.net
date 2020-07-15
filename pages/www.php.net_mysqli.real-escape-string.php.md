# mysqli::real_escape_string



Note, that if no connection is open, mysqli_real_escape_string() will return an empty string!  

#

For percent sign and underscore I use this:<br>

```
<?php
$more_escaped = addcslashes($escaped, '%_');
?>
```
  

#

Presenting several UTF-8 / Multibyte-aware escape functions.<br><br>These functions represent alternatives to mysqli::real_escape_string, as long as your DB connection and Multibyte extension are using the same character set (UTF-8), they will produce the same results by escaping the same characters as mysqli::real_escape_string.<br><br>This is based on research I did for my SQL Query Builder class:<br>https://github.com/twister

```
<?php/sql



```
<?php
/**
 * Returns a string with backslashes before characters that need to be escaped.
 * As required by MySQL and suitable for multi-byte character sets
 * Characters encoded are NUL (ASCII 0), \n, \r, \, ', ", and ctrl-Z.
 *
 * @param string $string String to add slashes to
 * @return $string with `\` prepended to reserved characters 
 *
 * @author Trevor Herselman
 */
if (function_exists('mb_ereg_replace'))
{
    function mb_escape(string $string)
    {
        return mb_ereg_replace('[\x00\x0A\x0D\x1A\x22\x27\x5C]', '\\\0', $string);
    }
} else {
    function mb_escape(string $string)
    {
        return preg_replace('~[\x00\x0A\x0D\x1A\x22\x27\x5C]~u', '\\\$0', $string);
    }
}

?>
```


Characters escaped are (the same as mysqli::real_escape_string):

00 = \0 (NUL)
0A = \n
0D = \r
1A = ctl-Z
22 = "
27 = '
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
 * Characters encoded are NUL (ASCII 0), \n, \r, \, ', ", and ctrl-Z.
 * In addition, the special control characters % and _ are also escaped,
 * suitable for all statements, but especially suitable for `LIKE`.
 *
 * @param string $string String to add slashes to
 * @return $string with `\` prepended to reserved characters 
 *
 * @author Trevor Herselman
 */
if (function_exists('mb_ereg_replace'))
{
    function mb_escape(string $string)
    {
        return mb_ereg_replace('[\x00\x0A\x0D\x1A\x22\x25\x27\x5C\x5F]', '\\\0', $string);
    }
} else {
    function mb_escape(string $string)
    {
        return preg_replace('~[\x00\x0A\x0D\x1A\x22\x25\x27\x5C\x5F]~u', '\\\$0', $string);
    }
}

?>
```


Additional characters escaped:

25 = %
5F = _

Bonus function:

The original MySQL `utf8` character-set (for tables and fields) only supports 3-byte sequences.
4-byte characters are not common, but I've had queries fail to execute on 4-byte UTF-8 characters, so you should be using `utf8mb4` wherever possible.

However, if you still want to use `utf8`, you can use the following function to replace all 4-byte sequences.



```
<?php
// Modified from: https://stackoverflow.com/a/24672780/2726557
function mysql_utf8_sanitizer(string $str)
{
    return preg_replace('/[\x{10000}-\x{10FFFF}]/u', "\xEF\xBF\xBD", $str);
}
?>
```
<br><br>Pick your poison and use at your own risk!  

#

You can avoid all character escaping issues (on the PHP side) if you use prepare() and bind_param(), as an alternative to placing arbitrary string values in SQL statements.  This works because bound parameter values are NOT passed via the SQL statement syntax.  

#

To escape for the purposes of having your queries made successfully, and to prevent SQLi (SQL injection)/stored and/or reflected XSS, it&apos;s a good idea to go with the basics first, then make sure nothing gets in that can be used for SQLi or stored/reflected XSS, or even worse, loading remote images and scripts.<br><br>For example:<br><br>

```
<?php
     
     // Assume this is a simple comments form with a name and comment.

     $name = mysqli_real_escape_string($conn, $_POST['name']);
     $comments = mysqli_real_escape_string($conn, $_POST['comments']);

     // Here is where most of the action happens.  But see note below
     // on dumping back out from the database

     // We should use the ENT_QUOTES flag second parameter...
     $name = htmlspecialchars($name);
     $comments = htmlspecialchars($comments);

     $insert_sql = "INSERT INTO tbl_comments ( c_id, c_name, c_comments ) VALUES ( DEFAULT, '" . $name . "', '" . $comments . "')";

     $res = mysqli_query($conn, $insert_sql);
     if ( $res === false ) {
          // Something went wrong, handle it
     }

     // Now output page showing comments
?>
```


//  Assume we're in a table with each row containing a name and comment



```
<?php
     
     $res = mysqli_query($conn, "SELECT c_name, c_comments FROM tbl_comments ORDER BY c_name ASC");

     if ( $res === false )
          // Something went wrong

     // Or as you like...
     while ( $row = mysqli_fetch_array($res, MYSQLI_BOTH) ) {
          
          // This will output safe HTML entities if they went in
          // They will be displayed, but not interpreted
          echo "<tr><td>" . $row['c_name'] . "</td>";
          echo "<td>" . $row['c_comments'] . "</td></tr>";

          // BUT, if you make this mistake...
          echo "<tr><td>" . htmlspecialchars_decode($row['c_name']) . "</td>";
          echo "<td>" . htmlspecialchars_decode($row['c_comments']) . "</td></tr>";

          // ... then your entities will reflect back as the characters, so
          // input such as this: "><img src=x onerror=alert('xss')>
          // will display the 'xss' in an alert box in the browser.
     }

     mysqli_free_result($res);
     mysqli_close($conn);
?>
```


In most cases, you wouldn't want to go way overboard sanitizing untrusted user input, for instance:



```
<?php
     $my_input = htmlspecialchars( strip_tags($_POST['foo']) );
?>
```
<br><br>This will junk a lot of input you might actually want, if you&apos;re rolling your own forum or comments section and it&apos;s for web developers, for example.  On the other hand, if legitimate users are never going to enter anything other than text, never HTML tags or anything else, it&apos;s not a bad idea.<br><br>The take-away is that mysqli_real_escape_string() is not good enough, and being overly-aggressive in sanitizing input may not be what you want.<br><br>Be aware that in the above example, it will protect you from sqli (run sqlmap on all your input fields and forms to check) but it won&apos;t protect your database from being filled with junk, effectively DoS&apos;ing your Web app in the process.<br><br>So after protecting against SQLi, even if you&apos;re behind CloudFlare and take other measures to protect your databases, there&apos;s still effectively a DoS attack that could slow down your Web App for legitimate users and make it a nightmare filled with rubbish that some poor maintainer has to clean out, if you don&apos;t take other measures.<br><br>So aside from escaping your stings, and protecting against SQLi and stored/reflected XSS, and maliciously loaded images or JS, there&apos;s also checking your input to see if it makes sense, so you don&apos;t get a database full of rubbish!<br><br>It just never ends... :-)  

#

Note that this function will NOT escape _ (underscore) and % (percent) signs, which have special meanings in LIKE clauses. <br><br>As far as I know there is no function to do this, so you have to escape them yourself by adding a backslash in front of them.  

#

When I submit data through Ajax I use a little function to reconvert the encoded chars to their original value. After that I do the escaping. Here the function:<br><br>   function my_htmlentities($input){<br>       $string = htmlentities($input,ENT_NOQUOTES,&apos;UTF-8&apos;);<br>       $string = str_replace(&apos;&amp;euro;&apos;,chr(128),$string);<br>       $string = html_entity_decode($string,ENT_NOQUOTES,&apos;ISO-8859-15&apos;);<br>       return $string;<br>   }<br><br>G.Zanferrari  

#

If you wonder why (besides \, &apos; and ")  NUL (ASCII 0), \n, \r, and Control-Z are escaped: it is not to prevent sql injection, but to prevent your sql logfile to get unreadable.  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.real-escape-string.php)

**[To root](/README.md)**