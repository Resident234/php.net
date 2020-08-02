# basename



Support of the $suffix parameter has changed between PHP4 and PHP5:<br>in PHP4, $suffix is removed first, and then the core basename is applied.<br>conversely, in PHP5, $suffix is removed AFTER applying core basename.<br><br>Example:<br>

```
<?php
  $file = "path/to/file.xml#xpointer(/Texture)";
  echo basename($file, ".xml#xpointer(/Texture)");
?>
```
<br><br>Result in PHP4: file<br>Result in PHP5: Texture)  

---

It&apos;s a shame, that for a 20 years of development we don&apos;t have mb_basename() yet!<br><br>// works both in windows and unix<br>function mb_basename($path) {<br>    if (preg_match(&apos;@^.*[\\\\/]([^\\\\/]+)$@s&apos;, $path, $matches)) {<br>        return $matches[1];<br>    } else if (preg_match(&apos;@^([^\\\\/]+)$@s&apos;, $path, $matches)) {<br>        return $matches[1];<br>    }<br>    return &apos;&apos;;<br>}  

---

There is only one variant that works in my case for my Russian UTF-8 letters:<br><br>

```
<?php
function mb_basename($file)
{
    return end(explode('/',$file));
}
><

It is intented for UNIX servers?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.basename.php)

**[To root](/README.md)**