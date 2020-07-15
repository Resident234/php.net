# mb_strlen



If you are unsure about what $encoding can be set to, here&apos;s a full list of all the encodings supported by this extension:<br><br>http://www.php.net/manual/en/mbstring.supported-encodings.php  

#

Speed of mb_strlen varies a lot according to specified character set.<br><br>If you need length of string in bytes (strlen cannot be trusted anymore because of mbstring.func_overload) you should use 

```
<?php mb_strlen($string, '8bit'); ?>
```
.<br>It&apos;s the fastest way (still a way slower than strlen, though) to determine byte length of string. Other single byte character sets (ASCII, ISO-8859-1, ...) are several times slower than 8bit.  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-strlen.php)

**[To root](/README.md)**