# mb_detect_encoding





If you try to use mb_detect_encoding to detect whether a string is valid UTF-8, use the strict mode, it is pretty worthless otherwise.



```
<?php
&#xA0; &#xA0; $str = &apos;&#xE1;&#xE9;&#xF3;&#xFA;&apos;; // ISO-8859-1
&#xA0; &#xA0; mb_detect_encoding($str, &apos;UTF-8&apos;); // &apos;UTF-8&apos;
&#xA0; &#xA0; mb_detect_encoding($str, &apos;UTF-8&apos;, true); // false
?>
```



  

#



If you need to distinguish between UTF-8 and ISO-8859-1 encoding, list UTF-8 first in your encoding_list:
mb_detect_encoding($string, &apos;UTF-8, ISO-8859-1&apos;);

if you list ISO-8859-1 first, mb_detect_encoding() will always return ISO-8859-1.

  

#



Based upon that snippet below using preg_match() I needed something faster and less specific.&#xA0; That function works and is brilliant but it scans the entire strings and checks that it conforms to UTF-8.&#xA0; I wanted something purely to check if a string contains UTF-8 characters so that I could switch character encoding from iso-8859-1 to utf-8.

I modified the pattern to only look for non-ascii multibyte sequences in the UTF-8 range and also to stop once it finds at least one multibytes string.&#xA0; This is quite a lot faster.



```
<?php

function detectUTF8($string)
{
&#xA0; &#xA0; &#xA0; &#xA0; return preg_match(&apos;%(?:
&#xA0; &#xA0; &#xA0; &#xA0; [\xC2-\xDF][\x80-\xBF]&#xA0; &#xA0; &#xA0; &#xA0; # non-overlong 2-byte
&#xA0; &#xA0; &#xA0; &#xA0; |\xE0[\xA0-\xBF][\x80-\xBF]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; # excluding overlongs
&#xA0; &#xA0; &#xA0; &#xA0; |[\xE1-\xEC\xEE\xEF][\x80-\xBF]{2}&#xA0; &#xA0; &#xA0; # straight 3-byte
&#xA0; &#xA0; &#xA0; &#xA0; |\xED[\x80-\x9F][\x80-\xBF]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; # excluding surrogates
&#xA0; &#xA0; &#xA0; &#xA0; |\xF0[\x90-\xBF][\x80-\xBF]{2}&#xA0; &#xA0; # planes 1-3
&#xA0; &#xA0; &#xA0; &#xA0; |[\xF1-\xF3][\x80-\xBF]{3}&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; # planes 4-15
&#xA0; &#xA0; &#xA0; &#xA0; |\xF4[\x80-\x8F][\x80-\xBF]{2}&#xA0; &#xA0; # plane 16
&#xA0; &#xA0; &#xA0; &#xA0; )+%xs&apos;, $string);
}

?>
```



  

#



Beware of bug to detect Russian encodings
http://bugs.php.net/bug.php?id=38138

  

#



A simple way to detect UTF-8/16/32 of file by its BOM (not work with string or file without BOM)



```
<?php
// Unicode BOM is U+FEFF, but after encoded, it will look like this.
define (&apos;UTF32_BIG_ENDIAN_BOM&apos;&#xA0;&#xA0; , chr(0x00) . chr(0x00) . chr(0xFE) . chr(0xFF));
define (&apos;UTF32_LITTLE_ENDIAN_BOM&apos;, chr(0xFF) . chr(0xFE) . chr(0x00) . chr(0x00));
define (&apos;UTF16_BIG_ENDIAN_BOM&apos;&#xA0;&#xA0; , chr(0xFE) . chr(0xFF));
define (&apos;UTF16_LITTLE_ENDIAN_BOM&apos;, chr(0xFF) . chr(0xFE));
define (&apos;UTF8_BOM&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; , chr(0xEF) . chr(0xBB) . chr(0xBF));

function detect_utf_encoding($filename) {

&#xA0; &#xA0; $text = file_get_contents($filename);
&#xA0; &#xA0; $first2 = substr($text, 0, 2);
&#xA0; &#xA0; $first3 = substr($text, 0, 3);
&#xA0; &#xA0; $first4 = substr($text, 0, 3);
&#xA0; &#xA0; 
&#xA0; &#xA0; if ($first3 == UTF8_BOM) return &apos;UTF-8&apos;;
&#xA0; &#xA0; elseif ($first4 == UTF32_BIG_ENDIAN_BOM) return &apos;UTF-32BE&apos;;
&#xA0; &#xA0; elseif ($first4 == UTF32_LITTLE_ENDIAN_BOM) return &apos;UTF-32LE&apos;;
&#xA0; &#xA0; elseif ($first2 == UTF16_BIG_ENDIAN_BOM) return &apos;UTF-16BE&apos;;
&#xA0; &#xA0; elseif ($first2 == UTF16_LITTLE_ENDIAN_BOM) return &apos;UTF-16LE&apos;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-detect-encoding.php)

**[To root](/README.md)**