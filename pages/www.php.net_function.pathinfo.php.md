# pathinfo





Simple example of pathinfo and array destructuring in PHP 7:


```
<?php
[ &apos;basename&apos; =&gt; $basename, &apos;dirname&apos; =&gt; $dirname ] = pathinfo(&apos;/www/htdocs/inc/lib.inc.php&apos;);

var_dump($basename, $dirname);

// result:
// string(11) &quot;lib.inc.php&quot;
// string(15) &quot;/www/htdocs/inc&quot;
?>
```



  

#



Note:

pathinfo() is locale aware, so for it to parse a path containing multibyte characters correctly, the matching locale must be set using the setlocale() function. 

Reality:
var_dump(pathinfo(&apos;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&apos;));
exit();
array(4) { &apos;dirname&apos; =&gt; string(1) &quot;.&quot; &apos;basename&apos; =&gt; string(8) &quot;2016.xls&quot; &apos;extension&apos; =&gt; string(3) &quot;xls&quot; &apos;filename&apos; =&gt; string(4) &quot;2016&quot; } 

Expect(Solve):
setlocale(LC_ALL, &apos;zh_CN.UTF-8&apos;);
var_dump(pathinfo(&apos;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&apos;));
exit();
array(4) { &apos;dirname&apos; =&gt; string(1) &quot;.&quot; &apos;basename&apos; =&gt; string(17) &quot;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&quot; &apos;extension&apos; =&gt; string(3) &quot;xls&quot; &apos;filename&apos; =&gt; string(13) &quot;&#x4E2D;&#x56FD;&#x4EBA;2016&quot; }

  

#

[Official documentation page](https://www.php.net/manual/en/function.pathinfo.php)

**[To root](/README.md)**