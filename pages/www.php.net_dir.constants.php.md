# Predefined Constants





In PHP 5.6 you can make a variadic function.



```
<?php
/**
 * Builds a file path with the appropriate directory separator.
 * @param string $segments,... unlimited number of path segments
 * @return string Path
 */
function file_build_path(...$segments) {
&#xA0; &#xA0; return join(DIRECTORY_SEPARATOR, $segments);
}

file_build_path(&quot;home&quot;, &quot;alice&quot;, &quot;Documents&quot;, &quot;example.txt&quot;);
?>
```


In earlier PHP versions you can use func_get_args.



```
<?php
function file_build_path() {
&#xA0; &#xA0; return join(DIRECTORY_SEPARATOR, func_get_args($segments));
}

file_build_path(&quot;home&quot;, &quot;alice&quot;, &quot;Documents&quot;, &quot;example.txt&quot;);
?>
```



  

#



For my part I&apos;ll continue to use this constant because it seems more future safe and flexible, even if Windows installations currently convert the paths magically. Not that syntax aesthetics matter but I think it can be made to look attractive:



```
<?php
$path = join(DIRECTORY_SEPARATOR, array(&apos;root&apos;, &apos;lib&apos;, &apos;file.php&apos;);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/dir.constants.php)

**[To root](/README.md)**