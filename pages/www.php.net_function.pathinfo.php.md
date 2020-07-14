# pathinfo



Simple example of pathinfo and array destructuring in PHP 7:<br>

```
<?php
[ &apos;basename&apos; =&gt; $basename, &apos;dirname&apos; =&gt; $dirname ] = pathinfo(&apos;/www/htdocs/inc/lib.inc.php&apos;);

var_dump($basename, $dirname);

// result:
// string(11) "lib.inc.php"
// string(15) "/www/htdocs/inc"
?>
```
  

#

Note:<br><br>pathinfo() is locale aware, so for it to parse a path containing multibyte characters correctly, the matching locale must be set using the setlocale() function. <br><br>Reality:<br>var_dump(pathinfo(&apos;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&apos;));<br>exit();<br>array(4) { &apos;dirname&apos; =&gt; string(1) "." &apos;basename&apos; =&gt; string(8) "2016.xls" &apos;extension&apos; =&gt; string(3) "xls" &apos;filename&apos; =&gt; string(4) "2016" } <br><br>Expect(Solve):<br>setlocale(LC_ALL, &apos;zh_CN.UTF-8&apos;);<br>var_dump(pathinfo(&apos;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&apos;));<br>exit();<br>array(4) { &apos;dirname&apos; =&gt; string(1) "." &apos;basename&apos; =&gt; string(17) "&#x4E2D;&#x56FD;&#x4EBA;2016.xls" &apos;extension&apos; =&gt; string(3) "xls" &apos;filename&apos; =&gt; string(13) "&#x4E2D;&#x56FD;&#x4EBA;2016" }  

#

[Official documentation page](https://www.php.net/manual/en/function.pathinfo.php)

**[To root](/README.md)**