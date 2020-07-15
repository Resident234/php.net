# rtrim



I have an obsessive love for php&apos;s array functions given how extremely easy they&apos;ve made complex string handling for me in various situations... so, have another string-rtrim() variant:<br><br>

```
<?php

function strrtrim($message, $strip) {
    // break message apart by strip string
    $lines = explode($strip, $message);
    $last  = '';
    // pop off empty strings at the end
    do {
        $last = array_pop($lines);
    } while (empty($last) &amp;&amp; (count($lines)));
    // re-assemble what remains
    return implode($strip, array_merge($lines, array($last)));
}

?>
```
<br><br>Astonishingly, something I didn&apos;t expect, but: It completely compares to harmor&apos;s rstrtrim below, execution time wise. o_o Whee!  

#

True, the Perl chomp() will only trim newline characters. There is, however, the Perl chop() function which is pretty much identical to the PHP rtrim()<br><br>---<br><br>Here&apos;s a quick way to recursively trim every element of an array, useful after the file() function :<br><br>

```
<?php
# Reads /etc/passwd file an trims newlines on each entry
$aFileContent = file("/etc/passwd");
foreach ($aFileContent as $sKey => $sValue) {
    $aFileContent[$sKey] = rtrim($sValue);
}

print_r($aFileContent);
?>
```
  

#

This shows how rtrim works when using the optional charlist parameter:<br>rtrim reads a character, one at a time, from the optional charlist parameter and compares it to the end of the str string. If the characters match, it trims it off and starts over again, looking at the "new" last character in the str string and compares it to the first character in the charlist again. If the characters do not match, it moves to the next character in the charlist parameter comparing once again. It continues until the charlist parameter has been completely processed, one at a time, and the str string no longer contains any matches. The newly "rtrimmed" string is returned.<br>

```
<?php
  // Example 1:
  rtrim('This is a short short sentence', 'short sentence');
  // returns 'This is a'
  // If you were expecting the result to be 'This is a short ',
  // then you're wrong; the exact string, 'short sentence',
  // isn't matched.  Remember, character-by-character comparison!
  // Example 2:
  rtrim('This is a short short sentence', 'cents');
  // returns 'This is a short short '
?>
```
  

#

On the recurring subject of string-stripping instead of character-stripping rtrim() implementations... the simplest (with a caveat) is probably the basename() function. It has a second parameter that functions as a right-trim using whole strings:<br><br>

```
<?php

echo basename('MooFoo', 'Foo');

?>
```


...outputs 'Moo'.

Since it also strips anything that looks like a directory, it's not quite identical with hacking a string off the end:



```
<?php

echo basename('Zoo/MooFoo', 'Foo');

?>
```
<br><br>...still outputs &apos;Moo&apos;.<br><br>But sometimes it gets the job done.  

#

I needed a way to trim all white space and then a few chosen strings from the end of a string.  So I wrote this class to reuse when stuff needs to be trimmed.  <br><br>

```
<?php

class cleaner {

function cleaner ($cuts,$pinfo) {
$ucut = "0";
$lcut = "0";
while ($cuts[$ucut]) {
$lcut++;
$ucut++;
}
$lcut = $lcut - 1;
$ucut = "0";
$rcut = "0";
$wiy = "start";

while ($wiy) {

if ($so) {
$ucut = "0";
$rcut = "0";
unset($so);
}

if (!$cuts[$ucut]) {
$so = "restart";
} else {
$pinfo = rtrim($pinfo);
$bpinfol = strlen($pinfo);
$tcut = $cuts[$ucut];
$pinfo = rtrim($pinfo,"$tcut");
$pinfol = strlen($pinfo);

    if ($bpinfol == $pinfol) {
    $rcut++;
    if ($rcut == $lcut) {
    unset($wiy);
    }
    $ucut++;
    } else {
    $so = "restart";
    }
}
}

$this->cleaner = $pinfo;
}

}

$pinfo = "Well... I'm really bored...&lt;br /&gt;&lt;br&gt;&amp;nbsp;    \n\t&amp;nbsp;&lt;br&gt;&lt;br /&gt;&lt;br&gt;&amp;nbsp;    \r\r&amp;nbsp;&lt;br&gt;\r&lt;br /&gt;&lt;br&gt;\r&amp;nbsp;    &amp;nbsp;\n&lt;br&gt;      &lt;br /&gt;\t";

$cuts = array('\n','\r','\t',' ',' ','&amp;nbsp;','&lt;br /&gt;','&lt;br&gt;','&lt;br/&gt;');

$pinfo = new cleaner($cuts,$pinfo);
$pinfo = $pinfo->cleaner;

print $pinfo;

?>
```
<br><br>That class will take any string that you put in the $cust array and remove it from the end of the $pinfo string.  It&apos;s useful for cleaning up comments, articles, or mail that users post to your site, making it so there&apos;s no extra blank space or blank lines.  

#

[Official documentation page](https://www.php.net/manual/en/function.rtrim.php)

**[To root](/README.md)**