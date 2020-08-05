# htmlspecialchars



As of PHP 5.4 they changed default encoding from "ISO-8859-1" to "UTF-8". So if you get null from htmlspecialchars or htmlentities<br><br>where you have only set <br>

```
<?php
echo htmlspecialchars($string);
echo htmlentities($string);
?>
```


you can fix it by


```
<?php
echo htmlspecialchars($string, ENT_COMPAT,'ISO-8859-1', true);
echo htmlentities($string, ENT_COMPAT,'ISO-8859-1', true);
?>
```
 <br><br>On linux you can find the scripts you need to fix by<br><br>grep -Rl "htmlspecialchars\\|htmlentities" /path/to/php/scripts/  

---

Unfortunately, as far as I can tell, the PHP devs did not provide ANY way to set the default encoding used by htmlspecialchars() or htmlentities(), even though they changed the default encoding in PHP 5.4 (*golf clap for PHP devs*). To save someone the time of trying it, this does not work:<br><br>

```
<?php
ini_set('default_charset', $charset); // doesn't work.
?>
```


Unfortunately, the only way to not have to explicitly provide the second and third parameter every single time this function is called (which gets extremely tedious) is to write your own function as a wrapper:



```
<?php
define('CHARSET', 'ISO-8859-1');
define('REPLACE_FLAGS', ENT_COMPAT | ENT_XHTML);

function html($string) {
    return htmlspecialchars($string, REPLACE_FLAGS, CHARSET);
}

echo html("&#xF1;"); // works
?>
```
<br><br>You can do the same for htmlentities()  

---

i searched for a while for a script, that could see the difference between an html tag and just &lt; and &gt; placed in the text, <br>the reason is that i recieve text from a database,<br>wich is inserted by an html form, and contains text and html tags, <br>the text can contain &lt; and &gt;, so does the tags,<br>with htmlspecialchars you can validate your text to XHTML,<br>but you&apos;ll also change the tags, like &lt;b&gt; to &amp;lt;b&amp;gt;,<br>so i needed a script that could see the difference between those two...<br>but i couldn&apos;t find one so i made my own one, <br>i havent fully tested it, but the parts i tested worked perfect!<br>just for people that were searching for something like this,<br>it may looks big, could be done easier, but it works for me, so im happy.<br><br>

```
<?php
function fixtags($text){
$text = htmlspecialchars($text);
$text = preg_replace("/=/", "=\"\"", $text);
$text = preg_replace("/&amp;quot;/", "&amp;quot;\"", $text);
$tags = "/&amp;lt;(\/|)(\w*)(\ |)(\w*)([\\\=]*)(?|(\")\"&amp;quot;\"|)(?|(.*)?&amp;quot;(\")|)([\ ]?)(\/|)&amp;gt;/i";
$replacement = "<$1$2$3$4$5$6$7$8$9$10>";
$text = preg_replace($tags, $replacement, $text);
$text = preg_replace("/=\"\"/", "=", $text);
return $text;
}
?>
```


an example:



```
<?php
$string = "
this is smaller < than this<br /> 
this is greater > than this<br />
this is the same = as this<br />
<a href=\"http://www.example.com/example.php?test=test\">This is a link</a><br />
<b>Bold</b> <i>italic</i> etc...";
echo fixtags($string);
?>
```
<br><br>will echo:<br>this is smaller &amp;lt; than this&lt;br /&gt; <br>this is greater &amp;gt; than this&lt;br /&gt; <br>this is the same = as this&lt;br /&gt; <br>&lt;a href="http://www.example.com/example.php?test=test"&gt;This is a link&lt;/a&gt;&lt;br /&gt; <br>&lt;b&gt;Bold&lt;/b&gt; &lt;i&gt;italic&lt;/i&gt; etc...<br><br>I hope its helpfull!!  

---

if your goal is just to protect your page from Cross Site Scripting (XSS) attack, or just to show HTML tags on a web page (showing &lt;body&gt; on the page, for example), then using htmlspecialchars() is good enough and better than using htmlentities().  A minor point is htmlspecialchars() is faster than htmlentities().  A more important point is, when we use  htmlspecialchars($s) in our code, it is automatically compatible with UTF-8 string.  Otherwise, if we use htmlentities($s), and there happens to be foreign characters in the string $s in UTF-8 encoding, then htmlentities() is going to mess it up, as it modifies the byte 0x80 to 0xFF in the string to entities like &amp;eacute;.  (unless you specifically provide a second argument and a third argument to htmlentities(), with the third argument being "UTF-8").<br><br>The reason htmlspecialchars($s) already works with UTF-8 string is that, it changes bytes that are in the range 0x00 to 0x7F to &amp;lt; etc, while leaving bytes in the range 0x80 to 0xFF unchanged.  We may wonder whether htmlspecialchars() may accidentally change any byte in a 2 to 4 byte UTF-8 character to &amp;lt; etc.  The answer is, it won&apos;t.  When a UTF-8 character is 2 to 4 bytes long, all the bytes in this character is in the 0x80 to 0xFF range. None can be in the 0x00 to 0x7F range.  When a UTF-8 character is 1 byte long, it is just the same as ASCII, which is 7 bit, from 0x00 to 0x7F.  As a result, when a UTF-8 character is 1 byte long, htmlspecialchars($s) will do its job, and when the UTF-8 character is 2 to 4 bytes long, htmlspecialchars($s) will just pass those bytes unchanged.  So htmlspecialchars($s) will do the same job no matter whether $s is in ASCII, ISO-8859-1 (Latin-1), or UTF-8.  

---

Be careful, the "charset" argument IS case sensitive. This is counter-intuitive and serves no practical purpose because the HTML spec actually has the opposite.  

---

[Official documentation page](https://www.php.net/manual/en/function.htmlspecialchars.php)

**[To root](/README.md)**