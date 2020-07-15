# Pattern Modifiers



Regarding the validity of a UTF-8 string when using the /u pattern modifier, some things to be aware of;<br><br>1. If the pattern itself contains an invalid UTF-8 character, you get an error (as mentioned in the docs above - "UTF-8 validity of the pattern is checked since PHP 4.3.5"<br><br>2. When the subject string contains invalid UTF-8 sequences / codepoints, it basically result in a "quiet death" for the preg_* functions, where nothing is matched but without indication that the string is invalid UTF-8<br><br>3. PCRE regards five and six octet UTF-8 character sequences as valid (both in patterns and the subject string) but these are not supported in Unicode ( see section 5.9 "Character Encoding" of the "Secure Programming for Linux and Unix HOWTO" - can be found at http://www.tldp.org/ and other places )<br><br>4. For an example algorithm in PHP which tests the validity of a UTF-8 string (and discards five / six octet sequences) head to: http://hsivonen.iki.fi/?>
```
utf8/<br><br>The following script should give you an idea of what works and what doesn&apos;t;<br><br>

```
<?php
$examples = array(
    'Valid ASCII' => "a",
    'Valid 2 Octet Sequence' => "\xc3\xb1",
    'Invalid 2 Octet Sequence' => "\xc3\x28",
    'Invalid Sequence Identifier' => "\xa0\xa1",
    'Valid 3 Octet Sequence' => "\xe2\x82\xa1",
    'Invalid 3 Octet Sequence (in 2nd Octet)' => "\xe2\x28\xa1",
    'Invalid 3 Octet Sequence (in 3rd Octet)' => "\xe2\x82\x28",

    'Valid 4 Octet Sequence' => "\xf0\x90\x8c\xbc",
    'Invalid 4 Octet Sequence (in 2nd Octet)' => "\xf0\x28\x8c\xbc",
    'Invalid 4 Octet Sequence (in 3rd Octet)' => "\xf0\x90\x28\xbc",
    'Invalid 4 Octet Sequence (in 4th Octet)' => "\xf0\x28\x8c\x28",
    'Valid 5 Octet Sequence (but not Unicode!)' => "\xf8\xa1\xa1\xa1\xa1",
    'Valid 6 Octet Sequence (but not Unicode!)' => "\xfc\xa1\xa1\xa1\xa1\xa1",
);

echo "++Invalid UTF-8 in pattern\n";
foreach ( $examples as $name => $str ) {
    echo "$name\n";
    preg_match("/".$str."/u",'Testing');
}

echo "++ preg_match() examples\n";
foreach ( $examples as $name => $str ) {
    
    preg_match("/\xf8\xa1\xa1\xa1\xa1/u", $str, $ar);
    echo "$name: ";

    if ( count($ar) == 0 ) {
        echo "Matched nothing!\n";
    } else {
        echo "Matched {$ar[0]}\n";
    }
    
}

echo "++ preg_match_all() examples\n";
foreach ( $examples as $name => $str ) {
    preg_match_all('/./u', $str, $ar);
    echo "$name: ";
    
    $num_utf8_chars = count($ar[0]);
    if ( $num_utf8_chars == 0 ) {
        echo "Matched nothing!\n";
    } else {
        echo "Matched $num_utf8_chars character\n";
    }
    
}
?>
```
  

#

The description of the "u" flag is a bit misleading. It suggests that it is only required if the pattern contains UTF-8 characters, when in fact it is required if either the pattern or the subject contain UTF-8. Without it, I was having problems with preg_match_all returning invalid multibyte characters when given a UTF-8 subject string.<br><br>It&apos;s fairly clear if you read the documentation for libpcre:<br><br>       In  order  process  UTF-8 strings, you must build PCRE to include UTF-8<br>       support in the code, and, in addition,  you  must  call  pcre_compile()<br>       with  the  PCRE_UTF8  option  flag,  or the pattern must start with the<br>       sequence (*UTF8). When either of these is the case,  both  the  pattern<br>       and  any  subject  strings  that  are matched against it are treated as<br>       UTF-8 strings instead of strings of 1-byte characters.<br><br>[from http://www.pcre.org/pcre.txt]  

#

[Official documentation page](https://www.php.net/manual/en/reference.pcre.pattern.modifiers.php)

**[To root](/README.md)**