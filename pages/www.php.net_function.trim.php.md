# trim



It may be useful to know that trim() returns an empty string when the argument is an unset/null variable.  

#

When specifying the character mask, <br>make sure that you use double quotes<br><br>

```
<?php
  $hello = " 
      Hello World   "; //here is a string with some trailing and leading whitespace

  $trimmed_correct   = trim($hello, " \t\n\r"); //<--------OKAY
  $trimmed_incorrect = trim($hello, ' \t\n\r'); //<--------NOT AS EXPECTED

  print("----------------------------");
  print("TRIMMED OK:".PHP_EOL);
  print_r($trimmed_correct.PHP_EOL);
  print("----------------------------");
  print("TRIMMING NOT OK:".PHP_EOL);
  print_r($trimmed_incorrect.PHP_EOL);
  print("----------------------------".PHP_EOL);
?>
```
<br><br>Here is the output:<br><br>----------------------------TRIMMED OK:<br>Hello World<br>----------------------------TRIMMING NOT OK:<br><br>      Hello World<br>----------------------------  

#

Non-breaking spaces can be troublesome with trim:<br><br>

```
<?php
// turn some HTML with non-breaking spaces into a "normal" string
$myHTML = "&amp;nbsp;abc";
$converted = strtr($myHTML, array_flip(get_html_translation_table(HTML_ENTITIES, ENT_QUOTES)));

// this WILL NOT work as expected
// $converted will still appear as " abc" in view source
// (but not in od -x)
$converted = trim($converted);

// &amp;nbsp; are translated to 0xA0, so use:
$converted = trim($converted, "\xA0"); // <- THIS DOES NOT WORK

// EDITED>>
// UTF encodes it as chr(0xC2).chr(0xA0)
$converted = trim($converted,chr(0xC2).chr(0xA0)); // should work

// PS: Thanks to John for saving my sanity!
?>
```
  

#

To remove multiple occurences of whitespace characters in a string an convert them all into single spaces, use this:<br><br>&lt;?<br><br>$text = preg_replace(&apos;/\s+/&apos;, &apos; &apos;, $text);<br><br>?>
```
<br><br>------------<br>JUBI<br>http://www.jubi.buum.pl  

#

Another way to trim all the elements of an array<br>

```
<?php
$newarray = array_map('trim', $array);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.trim.php)

**[To root](/README.md)**