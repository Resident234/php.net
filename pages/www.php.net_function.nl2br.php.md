# nl2br



It&apos;s important to remember that this function does NOT replace newlines with &lt;br&gt; tags. Rather, it inserts a &lt;br&gt; tag before each newline, but it still preserves the newlines themselves! This caused problems for me regarding a function I was writing -- I forgot the newlines were still being preserved.<br><br>If you don&apos;t want newlines, do:<br><br>

```
<?php
$Result = str_replace( "\n", '<br />', $Text );
?>
```
  

#

to replace all linebreaks to &lt;br /&gt;<br>the best solution (IMO) is:<br><br>

```
<?php
function nl2br2($string) {
$string = str_replace(array("\r\n", "\r", "\n"), "<br />", $string);
return $string;
}
?>
```
<br><br>because each OS have different ASCII chars for linebreak:<br>windows = \r\n<br>unix = \n<br>mac = \r<br><br>works perfect for me  

#

Here&apos;s a more simple one:<br><br>

```
<?php
/**
 * Convert BR tags to nl
 *
 * @param string The string to convert
 * @return string The converted string
 */
function br2nl($string)
{
    return preg_replace('/\<br(\s*)?\/?\>/i', "\n", $string);
}
?>
```
<br><br>Enjoy  

#

Starting from PHP 4.3.10 and PHP 5.0.2, this should be the most correct way to replace &lt;br /&gt; and &lt;br&gt; tags with newlines and carriage returns.<br>

```
<?php
/**
 * Convert BR tags to newlines and carriage returns.
 *
 * @param string The string to convert
 * @return string The converted string
 */
function br2nl ( $string )
{
    return preg_replace('/\<br(\s*)?\/?\>/i', PHP_EOL, $string);
}
?>
```

(Please note this is a minor edit of this function: http://php.net/nl2br#86678 )

You might also want to be "platform specific", and therefore this function might be of some help:


```
<?php
/**
 * Convert BR tags to newlines and carriage returns.
 *
 * @param string The string to convert
 * @param string The string to use as line separator
 * @return string The converted string
 */
function br2nl ( $string, $separator = PHP_EOL )
{
    $separator = in_array($separator, array("\n", "\r", "\r\n", "\n\r", chr(30), chr(155), PHP_EOL)) ? $separator : PHP_EOL;  // Checks if provided $separator is valid.
    return preg_replace('/\<br(\s*)?\/?\>/i', $separator, $string);
}
?>
```
  

#

Seeing all these suggestions on a br2nl function, I can also see that neither would work with a sloppy written html line break.. Users can&apos;t be trusted to write good code, we know that, and mixing case isn&apos;t too uncommon.<br><br>I think this little snippet would do most tricks, both XHTML style and HTML, even mixed case like &lt;Br&gt; &lt;bR /&gt; and even &lt;br            &gt; or &lt;br     /&gt;.<br><br>

```
<?php
function br2nl($text)
{
    return  preg_replace('/<br\\s*?\/??>
```
/i', '', $text);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.nl2br.php)

**[To root](/README.md)**