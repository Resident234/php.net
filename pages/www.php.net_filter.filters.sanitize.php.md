# Sanitize filters



FILTER_SANITIZE_STRING doesn&apos;t behavior the same as strip_tags function.    strip_tags allows less than symbol inferred from context, FILTER_SANITIZE_STRING strips regardless.<br>

```
<?php
$smaller = "not a tag &lt; 5";
echo strip_tags($smaller);    // -> not a tag &lt; 5
echo filter_var ( $smaller, FILTER_SANITIZE_STRING); // -> not a tag
?>
```
  

#

Remember to trim() the $_POST before your filters are applied:<br><br>

```
<?php

// We trim the $_POST data before any spaces get encoded to "%20"

// Trim array values using this function "trim_value"
function trim_value(&amp;$value)
{
    $value = trim($value);    // this removes whitespace and related characters from the beginning and end of the string
}
array_filter($_POST, 'trim_value');    // the data in $_POST is trimmed

$postfilter =    // set up the filters to be used with the trimmed post array
    array(
            'user_tasks'                        =>    array('filter' => FILTER_SANITIZE_STRING, 'flags' => !FILTER_FLAG_STRIP_LOW),    // removes tags. formatting code is encoded -- add nl2br() when displaying
            'username'                            =>    array('filter' => FILTER_SANITIZE_ENCODED, 'flags' => FILTER_FLAG_STRIP_LOW),    // we are using this in the url
            'mod_title'                            =>    array('filter' => FILTER_SANITIZE_ENCODED, 'flags' => FILTER_FLAG_STRIP_LOW),    // we are using this in the url
        );

$revised_post_array = filter_var_array($_POST, $postfilter);    // must be referenced via a variable which is now an array that takes the place of $_POST[]
echo (nl2br($revised_post_array['user_tasks']));    //-- use nl2br() upon output like so, for the ['user_tasks'] array value so that the newlines are formatted, since this is our HTML &lt;textarea&gt; field and we want to maintain newlines
?>
```
  

#

To include multiple flags, simply separate the flags with vertical pipe symbols.<br><br>For example, if you want to use filter_var() to sanitize $string with FILTER_SANITIZE_STRING and pass in FILTER_FLAG_STRIP_HIGH and FILTER_FLAG_STRIP_LOW, just call it like this:<br><br>$string = filter_var($string, FILTER_SANITIZE_STRING, FILTER_FLAG_STRIP_HIGH | FILTER_FLAG_STRIP_LOW);<br><br>The same goes for passing a flags field in an options array in the case of using callbacks.<br><br>$var = filter_var($string, FILTER_SANITIZE_SPECIAL_CHARS,<br>array(&apos;flags&apos; =&gt; FILTER_FLAG_STRIP_LOW | FILTER_FLAG_ENCODE_HIGH));<br><br>Thanks to the Brain Goo blog at popmartian.com/tipsntricks/for this info.  

#

It&apos;s not entirely clear what the LOW and HIGH ranges are. LOW is characters below 32, HIGH is those above 127, i.e. outside the ASCII range.<br><br>

```
<?php
$a = "\tcaf&#xE9;\n";
//This will remove the tab and the line break
echo filter_var($a, FILTER_SANITIZE_STRING, FILTER_FLAG_STRIP_LOW);
//This will remove the &#xE9;.
echo filter_var($a, FILTER_SANITIZE_STRING, FILTER_FLAG_STRIP_HIGH);
?>
```
  

#

Just to clarify, since this may be unknown for a lot of people:<br><br>ASCII characters above 127 are known as "Extended" and they represent characters such as greek letters and accented letters in latin alphabets, used in languages such as pt_BR.<br><br>A good ASCII quick reference (aside from the already mentioned Wikipedia article) can be found at: http://www.asciicodes.com/  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.sanitize.php)

**[To root](/README.md)**