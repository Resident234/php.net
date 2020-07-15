# preg_replace



Because i search a lot 4 this:<br><br>The following should be escaped if you are trying to match that character<br><br>\ ^ . $ | ( ) [ ]<br>* + ? { } ,<br><br>Special Character Definitions<br>\ Quote the next metacharacter<br>^ Match the beginning of the line<br>. Match any character (except newline)<br>$ Match the end of the line (or before newline at the end)<br>| Alternation<br>() Grouping<br>[] Character class<br>* Match 0 or more times<br>+ Match 1 or more times<br>? Match 1 or 0 times<br>{n} Match exactly n times<br>{n,} Match at least n times<br>{n,m} Match at least n but not more than m times<br>More Special Character Stuff<br>\t tab (HT, TAB)<br>\n newline (LF, NL)<br>\r return (CR)<br>\f form feed (FF)<br>\a alarm (bell) (BEL)<br>\e escape (think troff) (ESC)<br>\033 octal char (think of a PDP-11)<br>\x1B hex char<br>\c[ control char<br>\l lowercase next char (think vi)<br>\u uppercase next char (think vi)<br>\L lowercase till \E (think vi)<br>\U uppercase till \E (think vi)<br>\E end case modification (think vi)<br>\Q quote (disable) pattern metacharacters till \E<br>Even More Special Characters<br>\w Match a "word" character (alphanumeric plus "_")<br>\W Match a non-word character<br>\s Match a whitespace character<br>\S Match a non-whitespace character<br>\d Match a digit character<br>\D Match a non-digit character<br>\b Match a word boundary<br>\B Match a non-(word boundary)<br>\A Match only at beginning of string<br>\Z Match only at end of string, or before newline at the end<br>\z Match only at end of string<br>\G Match only where previous m//g left off (works only with /g)  

#

Post slug generator, for creating clean urls from titles.<br>It works with many languages.<br><br>

```
<?php
function remove_accent($str)
{
  $a = array('&#xC0;', '&#xC1;', '&#xC2;', '&#xC3;', '&#xC4;', '&#xC5;', '&#xC6;', '&#xC7;', '&#xC8;', '&#xC9;', '&#xCA;', '&#xCB;', '&#xCC;', '&#xCD;', '&#xCE;', '&#xCF;', '&#xD0;', '&#xD1;', '&#xD2;', '&#xD3;', '&#xD4;', '&#xD5;', '&#xD6;', '&#xD8;', '&#xD9;', '&#xDA;', '&#xDB;', '&#xDC;', '&#xDD;', '&#xDF;', '&#xE0;', '&#xE1;', '&#xE2;', '&#xE3;', '&#xE4;', '&#xE5;', '&#xE6;', '&#xE7;', '&#xE8;', '&#xE9;', '&#xEA;', '&#xEB;', '&#xEC;', '&#xED;', '&#xEE;', '&#xEF;', '&#xF1;', '&#xF2;', '&#xF3;', '&#xF4;', '&#xF5;', '&#xF6;', '&#xF8;', '&#xF9;', '&#xFA;', '&#xFB;', '&#xFC;', '&#xFD;', '&#xFF;', '&#x100;', '&#x101;', '&#x102;', '&#x103;', '&#x104;', '&#x105;', '&#x106;', '&#x107;', '&#x108;', '&#x109;', '&#x10A;', '&#x10B;', '&#x10C;', '&#x10D;', '&#x10E;', '&#x10F;', '&#x110;', '&#x111;', '&#x112;', '&#x113;', '&#x114;', '&#x115;', '&#x116;', '&#x117;', '&#x118;', '&#x119;', '&#x11A;', '&#x11B;', '&#x11C;', '&#x11D;', '&#x11E;', '&#x11F;', '&#x120;', '&#x121;', '&#x122;', '&#x123;', '&#x124;', '&#x125;', '&#x126;', '&#x127;', '&#x128;', '&#x129;', '&#x12A;', '&#x12B;', '&#x12C;', '&#x12D;', '&#x12E;', '&#x12F;', '&#x130;', '&#x131;', '&#x132;', '&#x133;', '&#x134;', '&#x135;', '&#x136;', '&#x137;', '&#x139;', '&#x13A;', '&#x13B;', '&#x13C;', '&#x13D;', '&#x13E;', '&#x13F;', '&#x140;', '&#x141;', '&#x142;', '&#x143;', '&#x144;', '&#x145;', '&#x146;', '&#x147;', '&#x148;', '&#x149;', '&#x14C;', '&#x14D;', '&#x14E;', '&#x14F;', '&#x150;', '&#x151;', '&#x152;', '&#x153;', '&#x154;', '&#x155;', '&#x156;', '&#x157;', '&#x158;', '&#x159;', '&#x15A;', '&#x15B;', '&#x15C;', '&#x15D;', '&#x15E;', '&#x15F;', '&#x160;', '&#x161;', '&#x162;', '&#x163;', '&#x164;', '&#x165;', '&#x166;', '&#x167;', '&#x168;', '&#x169;', '&#x16A;', '&#x16B;', '&#x16C;', '&#x16D;', '&#x16E;', '&#x16F;', '&#x170;', '&#x171;', '&#x172;', '&#x173;', '&#x174;', '&#x175;', '&#x176;', '&#x177;', '&#x178;', '&#x179;', '&#x17A;', '&#x17B;', '&#x17C;', '&#x17D;', '&#x17E;', '&#x17F;', '&#x192;', '&#x1A0;', '&#x1A1;', '&#x1AF;', '&#x1B0;', '&#x1CD;', '&#x1CE;', '&#x1CF;', '&#x1D0;', '&#x1D1;', '&#x1D2;', '&#x1D3;', '&#x1D4;', '&#x1D5;', '&#x1D6;', '&#x1D7;', '&#x1D8;', '&#x1D9;', '&#x1DA;', '&#x1DB;', '&#x1DC;', '&#x1FA;', '&#x1FB;', '&#x1FC;', '&#x1FD;', '&#x1FE;', '&#x1FF;');
  $b = array('A', 'A', 'A', 'A', 'A', 'A', 'AE', 'C', 'E', 'E', 'E', 'E', 'I', 'I', 'I', 'I', 'D', 'N', 'O', 'O', 'O', 'O', 'O', 'O', 'U', 'U', 'U', 'U', 'Y', 's', 'a', 'a', 'a', 'a', 'a', 'a', 'ae', 'c', 'e', 'e', 'e', 'e', 'i', 'i', 'i', 'i', 'n', 'o', 'o', 'o', 'o', 'o', 'o', 'u', 'u', 'u', 'u', 'y', 'y', 'A', 'a', 'A', 'a', 'A', 'a', 'C', 'c', 'C', 'c', 'C', 'c', 'C', 'c', 'D', 'd', 'D', 'd', 'E', 'e', 'E', 'e', 'E', 'e', 'E', 'e', 'E', 'e', 'G', 'g', 'G', 'g', 'G', 'g', 'G', 'g', 'H', 'h', 'H', 'h', 'I', 'i', 'I', 'i', 'I', 'i', 'I', 'i', 'I', 'i', 'IJ', 'ij', 'J', 'j', 'K', 'k', 'L', 'l', 'L', 'l', 'L', 'l', 'L', 'l', 'l', 'l', 'N', 'n', 'N', 'n', 'N', 'n', 'n', 'O', 'o', 'O', 'o', 'O', 'o', 'OE', 'oe', 'R', 'r', 'R', 'r', 'R', 'r', 'S', 's', 'S', 's', 'S', 's', 'S', 's', 'T', 't', 'T', 't', 'T', 't', 'U', 'u', 'U', 'u', 'U', 'u', 'U', 'u', 'U', 'u', 'U', 'u', 'W', 'w', 'Y', 'y', 'Y', 'Z', 'z', 'Z', 'z', 'Z', 'z', 's', 'f', 'O', 'o', 'U', 'u', 'A', 'a', 'I', 'i', 'O', 'o', 'U', 'u', 'U', 'u', 'U', 'u', 'U', 'u', 'U', 'u', 'A', 'a', 'AE', 'ae', 'O', 'o');
  return str_replace($a, $b, $str);
}

function post_slug($str)
{
  return strtolower(preg_replace(array('/[^a-zA-Z0-9 -]/', '/[ -]+/', '/^-|-$/'), 
  array('', '-', ''), remove_accent($str)));
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

$string_after = preg_replace( '/some_regexp/', "replacement", $string_before );

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

echo preg_replace_nth("/(\w+)\|/", '${1} is the 4th|', "|aa|b|cc|dd|e|ff|gg|kkk|", 4);

?>
```
<br><br>this outputs |aa|b|cc|dd is the 4th|e|ff|gg|kkk| <br>backreferences are accepted in $replacement  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-replace.php)

**[To root](/README.md)**