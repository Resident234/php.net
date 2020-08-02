# addslashes



@ mark at hagers dot demon dot nl :<br><br>You shouldn&apos;t use str_replace() for this. Use a function like htmlentities(), which will properly encode all user input for fields. That way, it will also work if the user types &amp;, &lt;, &gt;, etc.  

---

Never use addslashes function to escape values you are going to send to mysql. use mysql_real_escape_string or pg_escape at least if you are not using prepared queries yet.<br><br>keep in mind that single quote is not the only special character that can break your sql query. and quotes are the only thing which addslashes care.  

---

I was stumped for a long time by the fact that even when using addslashes and stripslashes explicitly on the field values double quotes (") still didn&apos;t seem to show up in strings read from a database. Until I looked at the source, and realised that the field value is just truncated at the first occurrence of a double quote. the remainder of the string is there (in the source), but is ignored when the form is displayed and submitted.<br><br>This can easily be solved by replacing double quotes with "&amp;quot;" when building the form. like this:<br>$fld_value =  str_replace ( "\"", "&amp;quot;", $src_string ) ;<br>The reverse replacement after the form submission is not necessary.  

---

[Official documentation page](https://www.php.net/manual/en/function.addslashes.php)

**[To root](/README.md)**