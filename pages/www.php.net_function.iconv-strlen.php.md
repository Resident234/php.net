# iconv_strlen





If iconv_strlen is passed a UTF-8 string containing badly formed sequences, it will return FALSE. This is in contrast to mb_strlen of the behaviour of utf8_decode, which strip out any bad sequences;



```
<?php
# UTF-8 string containing bad sequence: \xe9
$str = &quot;I&#xFFFD;t&#xFFFD;rn&#xFFFD;ti&#xFFFD;n\xe9&#xFFFD;liz&#xFFFD;ti&#xFFFD;n&quot;;

print &quot;mb_strlen: &quot;.mb_strlen($str,&apos;UTF-8&apos;).&quot;\n&quot;;
print &quot;strlen/utf8_decode: &quot;.strlen(utf8_decode($str)).&quot;\n&quot;;
print &quot;iconv_strlen: &quot;.iconv_strlen($str,&apos;UTF-8&apos;).&quot;\n&quot;;
?>
```


Displays;

mb_strlen: 20
strlen/utf8_decode: 20
iconv_strlen:

(PHP 5.0.5)

As such it is being &quot;stricter&quot; than mb_strlen and it may mean you need to check for invalid sequences first. A quick way to check is to exploit the behaviour of the PCRE extension (see notes on pattern modifiers);



```
<?php
if (preg_match(&apos;/^.{1}/us&apos;,$str,$ar) != 1) {
&#xA0; &#xA0; die(&quot;string contains invalid UTF-8&quot;);
}
?>
```


A slower but stricter check (regex) can be found at: http://www.w3.org/International/questions/qa-forms-utf-8

Similiar applies to iconv_substr, iconv_strpos and iconv_strrpos

  

#

[Official documentation page](https://www.php.net/manual/en/function.iconv-strlen.php)

**[To root](/README.md)**