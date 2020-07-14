# preg_replace



Because i search a lot 4 this:<br><br>The following should be escaped if you are trying to match that character<br><br>\ ^ . $ | ( ) [ ]<br>* + ? { } ,<br><br>Special Character Definitions<br>\ Quote the next metacharacter<br>^ Match the beginning of the line<br>. Match any character (except newline)<br>$ Match the end of the line (or before newline at the end)<br>| Alternation<br>() Grouping<br>[] Character class<br>* Match 0 or more times<br>+ Match 1 or more times<br>? Match 1 or 0 times<br>{n} Match exactly n times<br>{n,} Match at least n times<br>{n,m} Match at least n but not more than m times<br>More Special Character Stuff<br>\t tab (HT, TAB)<br>\n newline (LF, NL)<br>\r return (CR)<br>\f form feed (FF)<br>\a alarm (bell) (BEL)<br>\e escape (think troff) (ESC)<br>\033 octal char (think of a PDP-11)<br>\x1B hex char<br>\c[ control char<br>\l lowercase next char (think vi)<br>\u uppercase next char (think vi)<br>\L lowercase till \E (think vi)<br>\U uppercase till \E (think vi)<br>\E end case modification (think vi)<br>\Q quote (disable) pattern metacharacters till \E<br>Even More Special Characters<br>\w Match a "word" character (alphanumeric plus "_")<br>\W Match a non-word character<br>\s Match a whitespace character<br>\S Match a non-whitespace character<br>\d Match a digit character<br>\D Match a non-digit character<br>\b Match a word boundary<br>\B Match a non-(word boundary)<br>\A Match only at beginning of string<br>\Z Match only at end of string, or before newline at the end<br>\z Match only at end of string<br>\G Match only where previous m//g left off (works only with /g)  

#

Post slug generator, for creating clean urls from titles.<br>It works with many languages.<br><br>

```
<?php
function remove_accent($str)
{
  $a = array(&apos;&#xC0;&apos;, &apos;&#xC1;&apos;, &apos;&#xC2;&apos;, &apos;&#xC3;&apos;, &apos;&#xC4;&apos;, &apos;&#xC5;&apos;, &apos;&#xC6;&apos;, &apos;&#xC7;&apos;, &apos;&#xC8;&apos;, &apos;&#xC9;&apos;, &apos;&#xCA;&apos;, &apos;&#xCB;&apos;, &apos;&#xCC;&apos;, &apos;&#xCD;&apos;, &apos;&#xCE;&apos;, &apos;&#xCF;&apos;, &apos;&#xD0;&apos;, &apos;&#xD1;&apos;, &apos;&#xD2;&apos;, &apos;&#xD3;&apos;, &apos;&#xD4;&apos;, &apos;&#xD5;&apos;, &apos;&#xD6;&apos;, &apos;&#xD8;&apos;, &apos;&#xD9;&apos;, &apos;&#xDA;&apos;, &apos;&#xDB;&apos;, &apos;&#xDC;&apos;, &apos;&#xDD;&apos;, &apos;&#xDF;&apos;, &apos;&#xE0;&apos;, &apos;&#xE1;&apos;, &apos;&#xE2;&apos;, &apos;&#xE3;&apos;, &apos;&#xE4;&apos;, &apos;&#xE5;&apos;, &apos;&#xE6;&apos;, &apos;&#xE7;&apos;, &apos;&#xE8;&apos;, &apos;&#xE9;&apos;, &apos;&#xEA;&apos;, &apos;&#xEB;&apos;, &apos;&#xEC;&apos;, &apos;&#xED;&apos;, &apos;&#xEE;&apos;, &apos;&#xEF;&apos;, &apos;&#xF1;&apos;, &apos;&#xF2;&apos;, &apos;&#xF3;&apos;, &apos;&#xF4;&apos;, &apos;&#xF5;&apos;, &apos;&#xF6;&apos;, &apos;&#xF8;&apos;, &apos;&#xF9;&apos;, &apos;&#xFA;&apos;, &apos;&#xFB;&apos;, &apos;&#xFC;&apos;, &apos;&#xFD;&apos;, &apos;&#xFF;&apos;, &apos;&#x100;&apos;, &apos;&#x101;&apos;, &apos;&#x102;&apos;, &apos;&#x103;&apos;, &apos;&#x104;&apos;, &apos;&#x105;&apos;, &apos;&#x106;&apos;, &apos;&#x107;&apos;, &apos;&#x108;&apos;, &apos;&#x109;&apos;, &apos;&#x10A;&apos;, &apos;&#x10B;&apos;, &apos;&#x10C;&apos;, &apos;&#x10D;&apos;, &apos;&#x10E;&apos;, &apos;&#x10F;&apos;, &apos;&#x110;&apos;, &apos;&#x111;&apos;, &apos;&#x112;&apos;, &apos;&#x113;&apos;, &apos;&#x114;&apos;, &apos;&#x115;&apos;, &apos;&#x116;&apos;, &apos;&#x117;&apos;, &apos;&#x118;&apos;, &apos;&#x119;&apos;, &apos;&#x11A;&apos;, &apos;&#x11B;&apos;, &apos;&#x11C;&apos;, &apos;&#x11D;&apos;, &apos;&#x11E;&apos;, &apos;&#x11F;&apos;, &apos;&#x120;&apos;, &apos;&#x121;&apos;, &apos;&#x122;&apos;, &apos;&#x123;&apos;, &apos;&#x124;&apos;, &apos;&#x125;&apos;, &apos;&#x126;&apos;, &apos;&#x127;&apos;, &apos;&#x128;&apos;, &apos;&#x129;&apos;, &apos;&#x12A;&apos;, &apos;&#x12B;&apos;, &apos;&#x12C;&apos;, &apos;&#x12D;&apos;, &apos;&#x12E;&apos;, &apos;&#x12F;&apos;, &apos;&#x130;&apos;, &apos;&#x131;&apos;, &apos;&#x132;&apos;, &apos;&#x133;&apos;, &apos;&#x134;&apos;, &apos;&#x135;&apos;, &apos;&#x136;&apos;, &apos;&#x137;&apos;, &apos;&#x139;&apos;, &apos;&#x13A;&apos;, &apos;&#x13B;&apos;, &apos;&#x13C;&apos;, &apos;&#x13D;&apos;, &apos;&#x13E;&apos;, &apos;&#x13F;&apos;, &apos;&#x140;&apos;, &apos;&#x141;&apos;, &apos;&#x142;&apos;, &apos;&#x143;&apos;, &apos;&#x144;&apos;, &apos;&#x145;&apos;, &apos;&#x146;&apos;, &apos;&#x147;&apos;, &apos;&#x148;&apos;, &apos;&#x149;&apos;, &apos;&#x14C;&apos;, &apos;&#x14D;&apos;, &apos;&#x14E;&apos;, &apos;&#x14F;&apos;, &apos;&#x150;&apos;, &apos;&#x151;&apos;, &apos;&#x152;&apos;, &apos;&#x153;&apos;, &apos;&#x154;&apos;, &apos;&#x155;&apos;, &apos;&#x156;&apos;, &apos;&#x157;&apos;, &apos;&#x158;&apos;, &apos;&#x159;&apos;, &apos;&#x15A;&apos;, &apos;&#x15B;&apos;, &apos;&#x15C;&apos;, &apos;&#x15D;&apos;, &apos;&#x15E;&apos;, &apos;&#x15F;&apos;, &apos;&#x160;&apos;, &apos;&#x161;&apos;, &apos;&#x162;&apos;, &apos;&#x163;&apos;, &apos;&#x164;&apos;, &apos;&#x165;&apos;, &apos;&#x166;&apos;, &apos;&#x167;&apos;, &apos;&#x168;&apos;, &apos;&#x169;&apos;, &apos;&#x16A;&apos;, &apos;&#x16B;&apos;, &apos;&#x16C;&apos;, &apos;&#x16D;&apos;, &apos;&#x16E;&apos;, &apos;&#x16F;&apos;, &apos;&#x170;&apos;, &apos;&#x171;&apos;, &apos;&#x172;&apos;, &apos;&#x173;&apos;, &apos;&#x174;&apos;, &apos;&#x175;&apos;, &apos;&#x176;&apos;, &apos;&#x177;&apos;, &apos;&#x178;&apos;, &apos;&#x179;&apos;, &apos;&#x17A;&apos;, &apos;&#x17B;&apos;, &apos;&#x17C;&apos;, &apos;&#x17D;&apos;, &apos;&#x17E;&apos;, &apos;&#x17F;&apos;, &apos;&#x192;&apos;, &apos;&#x1A0;&apos;, &apos;&#x1A1;&apos;, &apos;&#x1AF;&apos;, &apos;&#x1B0;&apos;, &apos;&#x1CD;&apos;, &apos;&#x1CE;&apos;, &apos;&#x1CF;&apos;, &apos;&#x1D0;&apos;, &apos;&#x1D1;&apos;, &apos;&#x1D2;&apos;, &apos;&#x1D3;&apos;, &apos;&#x1D4;&apos;, &apos;&#x1D5;&apos;, &apos;&#x1D6;&apos;, &apos;&#x1D7;&apos;, &apos;&#x1D8;&apos;, &apos;&#x1D9;&apos;, &apos;&#x1DA;&apos;, &apos;&#x1DB;&apos;, &apos;&#x1DC;&apos;, &apos;&#x1FA;&apos;, &apos;&#x1FB;&apos;, &apos;&#x1FC;&apos;, &apos;&#x1FD;&apos;, &apos;&#x1FE;&apos;, &apos;&#x1FF;&apos;);
  $b = array(&apos;A&apos;, &apos;A&apos;, &apos;A&apos;, &apos;A&apos;, &apos;A&apos;, &apos;A&apos;, &apos;AE&apos;, &apos;C&apos;, &apos;E&apos;, &apos;E&apos;, &apos;E&apos;, &apos;E&apos;, &apos;I&apos;, &apos;I&apos;, &apos;I&apos;, &apos;I&apos;, &apos;D&apos;, &apos;N&apos;, &apos;O&apos;, &apos;O&apos;, &apos;O&apos;, &apos;O&apos;, &apos;O&apos;, &apos;O&apos;, &apos;U&apos;, &apos;U&apos;, &apos;U&apos;, &apos;U&apos;, &apos;Y&apos;, &apos;s&apos;, &apos;a&apos;, &apos;a&apos;, &apos;a&apos;, &apos;a&apos;, &apos;a&apos;, &apos;a&apos;, &apos;ae&apos;, &apos;c&apos;, &apos;e&apos;, &apos;e&apos;, &apos;e&apos;, &apos;e&apos;, &apos;i&apos;, &apos;i&apos;, &apos;i&apos;, &apos;i&apos;, &apos;n&apos;, &apos;o&apos;, &apos;o&apos;, &apos;o&apos;, &apos;o&apos;, &apos;o&apos;, &apos;o&apos;, &apos;u&apos;, &apos;u&apos;, &apos;u&apos;, &apos;u&apos;, &apos;y&apos;, &apos;y&apos;, &apos;A&apos;, &apos;a&apos;, &apos;A&apos;, &apos;a&apos;, &apos;A&apos;, &apos;a&apos;, &apos;C&apos;, &apos;c&apos;, &apos;C&apos;, &apos;c&apos;, &apos;C&apos;, &apos;c&apos;, &apos;C&apos;, &apos;c&apos;, &apos;D&apos;, &apos;d&apos;, &apos;D&apos;, &apos;d&apos;, &apos;E&apos;, &apos;e&apos;, &apos;E&apos;, &apos;e&apos;, &apos;E&apos;, &apos;e&apos;, &apos;E&apos;, &apos;e&apos;, &apos;E&apos;, &apos;e&apos;, &apos;G&apos;, &apos;g&apos;, &apos;G&apos;, &apos;g&apos;, &apos;G&apos;, &apos;g&apos;, &apos;G&apos;, &apos;g&apos;, &apos;H&apos;, &apos;h&apos;, &apos;H&apos;, &apos;h&apos;, &apos;I&apos;, &apos;i&apos;, &apos;I&apos;, &apos;i&apos;, &apos;I&apos;, &apos;i&apos;, &apos;I&apos;, &apos;i&apos;, &apos;I&apos;, &apos;i&apos;, &apos;IJ&apos;, &apos;ij&apos;, &apos;J&apos;, &apos;j&apos;, &apos;K&apos;, &apos;k&apos;, &apos;L&apos;, &apos;l&apos;, &apos;L&apos;, &apos;l&apos;, &apos;L&apos;, &apos;l&apos;, &apos;L&apos;, &apos;l&apos;, &apos;l&apos;, &apos;l&apos;, &apos;N&apos;, &apos;n&apos;, &apos;N&apos;, &apos;n&apos;, &apos;N&apos;, &apos;n&apos;, &apos;n&apos;, &apos;O&apos;, &apos;o&apos;, &apos;O&apos;, &apos;o&apos;, &apos;O&apos;, &apos;o&apos;, &apos;OE&apos;, &apos;oe&apos;, &apos;R&apos;, &apos;r&apos;, &apos;R&apos;, &apos;r&apos;, &apos;R&apos;, &apos;r&apos;, &apos;S&apos;, &apos;s&apos;, &apos;S&apos;, &apos;s&apos;, &apos;S&apos;, &apos;s&apos;, &apos;S&apos;, &apos;s&apos;, &apos;T&apos;, &apos;t&apos;, &apos;T&apos;, &apos;t&apos;, &apos;T&apos;, &apos;t&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;W&apos;, &apos;w&apos;, &apos;Y&apos;, &apos;y&apos;, &apos;Y&apos;, &apos;Z&apos;, &apos;z&apos;, &apos;Z&apos;, &apos;z&apos;, &apos;Z&apos;, &apos;z&apos;, &apos;s&apos;, &apos;f&apos;, &apos;O&apos;, &apos;o&apos;, &apos;U&apos;, &apos;u&apos;, &apos;A&apos;, &apos;a&apos;, &apos;I&apos;, &apos;i&apos;, &apos;O&apos;, &apos;o&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;U&apos;, &apos;u&apos;, &apos;A&apos;, &apos;a&apos;, &apos;AE&apos;, &apos;ae&apos;, &apos;O&apos;, &apos;o&apos;);
  return str_replace($a, $b, $str);
}

function post_slug($str)
{
  return strtolower(preg_replace(array(&apos;/[^a-zA-Z0-9 -]/&apos;, &apos;/[ -]+/&apos;, &apos;/^-|-$/&apos;), 
  array(&apos;&apos;, &apos;-&apos;, &apos;&apos;), remove_accent($str)));
}
?>
```
<br><br>Example: post_slug(&apos; -Lo#&amp;@rem  IPSUM //dolor-/sit - amet-/-consectetur! 12 -- &apos;)<br>will output: lorem-ipsum-dolor-sit-amet-consectetur-12  

#

If you want to catch characters, as well european, russian, chinese, japanese, korean of whatever, just :<br>- use mb_internal_encoding(&apos;UTF-8&apos;);<br>- use preg_replace(&apos;`...`u&apos;, &apos;...&apos;, $string) with the u (unicode) modifier<br><br>For further information, the complete list of preg_* modifiers could be found at :<br>http://php.net/manual/en/reference.pcre.pattern.modifiers.php  

#

preg_replace (and other preg-functions) return null instead of a string when encountering problems you probably did not think about!<br>-------------------------<br><br>It may not be obvious to everybody that the function returns NULL if an error of any kind occurres. An error I happen to stumple about quite often was the back-tracking-limit:<br>http://de.php.net/manual/de/pcre.configuration.php<br>#ini.pcre.backtrack-limit<br><br>When working with HTML-documents and their parsing it happens that you encounter documents that have a length of over 100.000 characters and that may lead to certain regular-expressions to fail due the back-tracking-limit of above.<br><br>A regular-expression that is ungreedy ("U", http://de.php.net/manual/de/reference.pcre.pattern.modifiers.php) often does the job, but still: sometimes you just need a greedy regular expression working on long strings ...<br><br>Since, an unhandled return-value of NULL usually creates a consecutive error in the application with unwanted and unforeseen consequences, I found the following solution to be quite helpful and at least save the application from crashing:<br><br>

```
<?php

$string_after = preg_replace( &apos;/some_regexp/&apos;, "replacement", $string_before );

// if some error occurred we go on working with the unchanged original string
if (PREG_NO_ERROR !== preg_last_error())
{
    $string_after = $string_before;
    
    // put email-sending or a log-message here
} //if

// free memory
unset( $string_before );

?>
```
<br><br>You may or should also put a log-message or the sending of an email into the if-condition in order to get informed, once, one of your regular-expressions does not have the effect you desired it to have.  

#

If you want to replace only the n-th occurrence of $pattern, you can use this function:<br><br>

```
<?php

function preg_replace_nth($pattern, $replacement, $subject, $nth=1) {
    return preg_replace_callback($pattern,
        function($found) use (&amp;$pattern, &amp;$replacement, &amp;$nth) {
                $nth--;
                if ($nth==0) return preg_replace($pattern, $replacement, reset($found) );
                return reset($found);
        }, $subject,$nth  );
}

echo preg_replace_nth("/(\w+)\|/", &apos;${1} is the 4th|&apos;, "|aa|b|cc|dd|e|ff|gg|kkk|", 4);

?>
```
<br><br>this outputs |aa|b|cc|dd is the 4th|e|ff|gg|kkk| <br>backreferences are accepted in $replacement  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-replace.php)

**[To root](/README.md)**