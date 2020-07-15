# strcmp



If you rely on strcmp for safe string comparisons, both parameters must be strings, the result is otherwise extremely unpredictable.<br>For instance you may get an unexpected 0, or return values of NULL, -2, 2, 3 and -3.<br><br>strcmp("5", 5) =&gt; 0<br>strcmp("15", 0xf) =&gt; 0<br>strcmp(61529519452809720693702583126814, 61529519452809720000000000000000) =&gt; 0<br>strcmp(NULL, false) =&gt; 0<br>strcmp(NULL, "") =&gt; 0<br>strcmp(NULL, 0) =&gt; -1<br>strcmp(false, -1) =&gt; -2<br>strcmp("15", NULL) =&gt; 2<br>strcmp(NULL, "foo") =&gt; -3<br>strcmp("foo", NULL) =&gt; 3<br>strcmp("foo", false) =&gt; 3<br>strcmp("foo", 0) =&gt; 1<br>strcmp("foo", 5) =&gt; 1<br>strcmp("foo", array()) =&gt; NULL + PHP Warning<br>strcmp("foo", new stdClass) =&gt; NULL + PHP Warning<br>strcmp(function(){}, "") =&gt; NULL + PHP Warning  

#

i hope this will give you a clear idea how strcmp works internally.<br><br>

```
<?php
$str1 = "b";
echo ord($str1); //98
echo "&lt;br/&gt;";
$str2 = "t";
echo ord($str2); //116
echo "&lt;br/&gt;";
echo ord($str1)-ord($str2);//-18
$str1 = "bear";
$str2 = "tear";
$str3 = "";
echo "&lt;pre&gt;";
echo strcmp($str1, $str2); // -18
echo "&lt;br/&gt;";
echo strcmp($str2, $str1); //18
echo "&lt;br/&gt;";
echo strcmp($str2, $str2); //0
echo "&lt;br/&gt;";
echo strcmp($str2, $str3); //4
echo "&lt;br/&gt;";
echo strcmp($str3, $str2); //-4
echo "&lt;br/&gt;";
echo strcmp($str3, $str3); // 0
echo "&lt;/pre&gt;";
?>
```
  

#

One big caveat - strings retrieved from the backtick operation may be zero terminated (C-style), and therefore will not be equal to the non-zero terminated strings (roughly Pascal-style) normal in PHP. The workaround is to surround every `` pair or shell_exec() function with the trim() function. This is likely to be an issue with other functions that invoke shells; I haven&apos;t bothered to check.<br><br>On Debian Lenny (and RHEL 5, with minor differences), I get this:<br><br>====PHP====<br>

```
<?php
$sz = `pwd`;
$ps = "/var/www";

echo "Zero-terminated string:&lt;br /&gt;sz = ".$sz."&lt;br /&gt;str_split(sz) = "; print_r(str_split($sz));
echo "&lt;br /&gt;&lt;br /&gt;";

echo "Pascal-style string:&lt;br /&gt;ps = ".$ps."&lt;br /&gt;str_split(ps) = "; print_r(str_split($ps));
echo "&lt;br /&gt;&lt;br /&gt;";

echo "Normal results of comparison:&lt;br /&gt;";
echo "sz == ps = ".($sz == $ps ? "true" : "false")."&lt;br /&gt;";
echo "strcmp(sz,ps) = ".strcmp($sz,$ps);
echo "&lt;br /&gt;&lt;br /&gt;";

echo "Comparison with trim()'d zero-terminated string:&lt;br /&gt;";
echo "trim(sz) = ".trim($sz)."&lt;br /&gt;";
echo "str_split(trim(sz)) = "; print_r(str_split(trim($sz))); echo "&lt;br /&gt;";
echo "trim(sz) == ps = ".(trim($sz) == $ps ? "true" : "false")."&lt;br /&gt;";
echo "strcmp(trim(sz),ps) = ".strcmp(trim($sz),$ps);
?>
```
<br><br>====Output====<br>Zero-terminated string:<br>sz = /var/www <br>str_split(sz) = Array ( [0] =&gt; / [1] =&gt; v [2] =&gt; a [3] =&gt; r [4] =&gt; / [5] =&gt; w [6] =&gt; w [7] =&gt; w [8] =&gt; ) <br><br>Pascal-style string:<br>ps = /var/www<br>str_split(ps) = Array ( [0] =&gt; / [1] =&gt; v [2] =&gt; a [3] =&gt; r [4] =&gt; / [5] =&gt; w [6] =&gt; w [7] =&gt; w ) <br><br>Normal results of comparison:<br>sz == ps = false<br>strcmp(sz,ps) = 1<br><br>Comparison with trim()&apos;d zero-terminated string:<br>trim(sz) = /var/www<br>str_split(trim(sz)) = Array ( [0] =&gt; / [1] =&gt; v [2] =&gt; a [3] =&gt; r [4] =&gt; / [5] =&gt; w [6] =&gt; w [7] =&gt; w ) <br>trim(sz) == ps = true<br>strcmp(trim(sz),ps) = 0  

#

Hey be sure the string you are comparing has not special characters like &apos;\n&apos; or something like that.  

#

[Official documentation page](https://www.php.net/manual/en/function.strcmp.php)

**[To root](/README.md)**