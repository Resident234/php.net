# $_GET



Just a note, because I didn&apos;t know for sure until I tested it.<br><br>If you have a query string that contains a parameter but no value (not even an equals sign), like so:<br>http://path/to/script.php?a<br><br>The following script is a good test to determine how a is valued:<br>&lt;pre&gt;<br>

```
<?php
 print_r($_GET);
 if($_GET["a"] === "") echo "a is an empty string\n";
 if($_GET["a"] === false) echo "a is false\n";
 if($_GET["a"] === null) echo "a is null\n";
 if(isset($_GET["a"])) echo "a is set\n";
 if(!empty($_GET["a"])) echo "a is not empty";
?>
```
<br>&lt;/pre&gt;<br><br>I tested this with script.php?a, and it returned:<br><br>a is an empty string<br>a is set<br><br>So note that a parameter with no value associated with, even without an equals sign, is considered to be an empty string (""), isset() returns true for it, and it is considered empty, but not false or null. Seems obvious after the first test, but I just had to make sure.<br><br>Of course, if I do not include it in my browser query, the script returns<br>Array<br>(<br>)<br>a is null  

---

Note that named anchors are not part of the query string and are never submitted by the browser to the server.<br><br>Eg.<br>http://www.xyz-abc.kz/index.php?title=apocalypse.php#doom<br><br>echo $_GET[&apos;title&apos;]; <br><br>// returns "apocalypse.php" and NOT "apocalypse.php#doom"<br><br>you would be better off treating the named anchor as another query string variable like so:<br>http://www.xyz-abc.kz/index.php?title=apocalypse.php&amp;na=doom<br><br>...and then retrieve it using something like this:<br>$url = $_GET[&apos;title&apos;]."#".$_GET[&apos;na&apos;];<br><br>Hope this helps someone...  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.get.php)

**[To root](/README.md)**