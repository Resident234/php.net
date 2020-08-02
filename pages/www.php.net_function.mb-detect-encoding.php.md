# mb_detect_encoding



If you try to use mb_detect_encoding to detect whether a string is valid UTF-8, use the strict mode, it is pretty worthless otherwise.<br><br>

```
<?php
    $str = '&#xE1;&#xE9;&#xF3;&#xFA;'; // ISO-8859-1
    mb_detect_encoding($str, 'UTF-8'); // 'UTF-8'
    mb_detect_encoding($str, 'UTF-8', true); // false
?>
```
  

---

If you need to distinguish between UTF-8 and ISO-8859-1 encoding, list UTF-8 first in your encoding_list:<br>mb_detect_encoding($string, &apos;UTF-8, ISO-8859-1&apos;);<br><br>if you list ISO-8859-1 first, mb_detect_encoding() will always return ISO-8859-1.  

---

Based upon that snippet below using preg_match() I needed something faster and less specific.  That function works and is brilliant but it scans the entire strings and checks that it conforms to UTF-8.  I wanted something purely to check if a string contains UTF-8 characters so that I could switch character encoding from iso-8859-1 to utf-8.<br><br>I modified the pattern to only look for non-ascii multibyte sequences in the UTF-8 range and also to stop once it finds at least one multibytes string.  This is quite a lot faster.<br><br>

```
<?php

function detectUTF8($string)
{
        return preg_match('%(?:
        [\xC2-\xDF][\x80-\xBF]        # non-overlong 2-byte
        |\xE0[\xA0-\xBF][\x80-\xBF]               # excluding overlongs
        |[\xE1-\xEC\xEE\xEF][\x80-\xBF]{2}      # straight 3-byte
        |\xED[\x80-\x9F][\x80-\xBF]               # excluding surrogates
        |\xF0[\x90-\xBF][\x80-\xBF]{2}    # planes 1-3
        |[\xF1-\xF3][\x80-\xBF]{3}                  # planes 4-15
        |\xF4[\x80-\x8F][\x80-\xBF]{2}    # plane 16
        )+%xs', $string);
}

?>
```
  

---

Beware of bug to detect Russian encodings<br>http://bugs.php.net/bug.php?id=38138  

---

A simple way to detect UTF-8/16/32 of file by its BOM (not work with string or file without BOM)<br><br>

```
<?php
// Unicode BOM is U+FEFF, but after encoded, it will look like this.
define ('UTF32_BIG_ENDIAN_BOM'   , chr(0x00) . chr(0x00) . chr(0xFE) . chr(0xFF));
define ('UTF32_LITTLE_ENDIAN_BOM', chr(0xFF) . chr(0xFE) . chr(0x00) . chr(0x00));
define ('UTF16_BIG_ENDIAN_BOM'   , chr(0xFE) . chr(0xFF));
define ('UTF16_LITTLE_ENDIAN_BOM', chr(0xFF) . chr(0xFE));
define ('UTF8_BOM'               , chr(0xEF) . chr(0xBB) . chr(0xBF));

function detect_utf_encoding($filename) {

    $text = file_get_contents($filename);
    $first2 = substr($text, 0, 2);
    $first3 = substr($text, 0, 3);
    $first4 = substr($text, 0, 3);
    
    if ($first3 == UTF8_BOM) return 'UTF-8';
    elseif ($first4 == UTF32_BIG_ENDIAN_BOM) return 'UTF-32BE';
    elseif ($first4 == UTF32_LITTLE_ENDIAN_BOM) return 'UTF-32LE';
    elseif ($first2 == UTF16_BIG_ENDIAN_BOM) return 'UTF-16BE';
    elseif ($first2 == UTF16_LITTLE_ENDIAN_BOM) return 'UTF-16LE';
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.mb-detect-encoding.php)

**[To root](/README.md)**