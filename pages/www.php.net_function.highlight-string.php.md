# highlight_string



You can change the colors of the highlighting, like this: <br><br>

```
<?php
ini_set("highlight.comment", "#008000");
ini_set("highlight.default", "#000000");
ini_set("highlight.html", "#808080");
ini_set("highlight.keyword", "#0000BB; font-weight: bold");
ini_set("highlight.string", "#DD0000");
?>
```


Like you see in the example above, you can even add additional styles like bold text, since the values are set directly to the DOM attribute "style".

Also, this function highlights only text, if it begins with the prefix ``<?php``. But this function can highlight other similar formats too (not perfectly, but better than nothing), like HTML, XML, C++, JavaScript, etc. I use following function to highlight different file types and it works quite good:



```
<?php
function highlightText($text)
{
    $text = trim($text);
    $text = highlight_string("<?php " . $text, true);  // highlight_string() requires opening PHP tag or otherwise it will not colorize the text
    $text = trim($text);
    $text = preg_replace("|^\\<code\\>\\<span style\\=\"color\\: #[a-fA-F0-9]{0,6}\"\\>|", "", $text, 1);  // remove prefix
    $text = preg_replace("|\\</code\\>\$|", "", $text, 1);  // remove suffix 1
    $text = trim($text);  // remove line breaks
    $text = preg_replace("|\\</span\\>\$|", "", $text, 1);  // remove suffix 2
    $text = trim($text);  // remove line breaks
    $text = preg_replace("|^(\\<span style\\=\"color\\: #[a-fA-F0-9]{0,6}\"\\>)(&amp;lt;\\?php&amp;nbsp;)(.*?)(\\</span\\>)|", "\$1\$3\$4", $text);  // remove custom added "<?php "

    return $text;
}
?>
```


Note, that it will remove the ``<code>`` tag too, so you get the formatted text directly, which gives you more freedom to work with the result.

I personally suggest to combine both things to have a nice highlighting function for different file types with different highlight coloring sets:



```
<?php
function highlightText($text, $fileExt="")
{
    if ($fileExt == "php")
    {
        ini_set("highlight.comment", "#008000");
        ini_set("highlight.default", "#000000");
        ini_set("highlight.html", "#808080");
        ini_set("highlight.keyword", "#0000BB; font-weight: bold");
        ini_set("highlight.string", "#DD0000");
    }
    else if ($fileExt == "html")
    {
        ini_set("highlight.comment", "green");
        ini_set("highlight.default", "#CC0000");
        ini_set("highlight.html", "#000000");
        ini_set("highlight.keyword", "black; font-weight: bold");
        ini_set("highlight.string", "#0000FF");
    }
    // ...

    $text = trim($text);
    $text = highlight_string("<?php " . $text, true);  // highlight_string() requires opening PHP tag or otherwise it will not colorize the text
    $text = trim($text);
    $text = preg_replace("|^\\<code\\>\\<span style\\=\"color\\: #[a-fA-F0-9]{0,6}\"\\>|", "", $text, 1);  // remove prefix
    $text = preg_replace("|\\</code\\>\$|", "", $text, 1);  // remove suffix 1
    $text = trim($text);  // remove line breaks
    $text = preg_replace("|\\</span\\>\$|", "", $text, 1);  // remove suffix 2
    $text = trim($text);  // remove line breaks
    $text = preg_replace("|^(\\<span style\\=\"color\\: #[a-fA-F0-9]{0,6}\"\\>)(&amp;lt;\\?php&amp;nbsp;)(.*?)(\\</span\\>)|", "\$1\$3\$4", $text);  // remove custom added "<?php "

    return $text;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.highlight-string.php)

**[To root](/README.md)**