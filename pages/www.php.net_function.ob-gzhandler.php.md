# ob_gzhandler



I&apos;ve just spent 5 hours fighting a bug in my application and outcome is:<br><br>

```
<?php
// do not use
ob_start("ob_gzhandler");
// along with
header('HTTP/1.1 304 Not Modified');

// or in the end use
ob_end_clean();
?>
```
<br><br>W3C Standart requires response body to be empty if 304 header set. With compression on it will at least contain a gzip stream header even if your output is completely empty! <br><br>This affects firefox (current ver.3.6.3) in a very subtle way: one of the requests after the one that gets 304 with not empty body gets it response prepended with contents of that body. In my case it was a css file and styles was not rendered at all, which made problem show up.  

---

[Official documentation page](https://www.php.net/manual/en/function.ob-gzhandler.php)

**[To root](/README.md)**