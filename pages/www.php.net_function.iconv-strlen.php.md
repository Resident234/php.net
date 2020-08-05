# iconv_strlen



If iconv_strlen is passed a UTF-8 string containing badly formed sequences, it will return FALSE. This is in contrast to mb_strlen of the behaviour of utf8_decode, which strip out any bad sequences;<br><br>

```
<?php
# UTF-8 string containing bad sequence: \xe9
$str = "I&#xFFFD;t&#xFFFD;rn&#xFFFD;ti&#xFFFD;n\xe9&#xFFFD;liz&#xFFFD;ti&#xFFFD;n";

print "mb_strlen: ".mb_strlen($str,'UTF-8')."\n";
print "strlen/utf8_decode: ".strlen(utf8_decode($str))."\n";
print "iconv_strlen: ".iconv_strlen($str,'UTF-8')."\n";
?>
```


Displays;

mb_strlen: 20
strlen/utf8_decode: 20
iconv_strlen:

(PHP 5.0.5)

As such it is being "stricter" than mb_strlen and it may mean you need to check for invalid sequences first. A quick way to check is to exploit the behaviour of the PCRE extension (see notes on pattern modifiers);



```
<?php
if (preg_match('/^.{1}/us',$str,$ar) != 1) {
    die("string contains invalid UTF-8");
}
?>
```
<br><br>A slower but stricter check (regex) can be found at: http://www.w3.org/International/questions/qa-forms-utf-8<br><br>Similiar applies to iconv_substr, iconv_strpos and iconv_strrpos  

---

[Official documentation page](https://www.php.net/manual/en/function.iconv-strlen.php)

**[To root](/README.md)**