# realpath





Because realpath() does not work on files that do not
exist, I wrote a function that does.
It replaces (consecutive) occurences of / and \\ with
whatever is in DIRECTORY_SEPARATOR, and processes /. and /.. fine.
Paths returned by get_absolute_path() contain no
(back)slash at position 0 (beginning of the string) or
position -1 (ending)


```
<?php
&#xA0; &#xA0; function get_absolute_path($path) {
&#xA0; &#xA0; &#xA0; &#xA0; $path = str_replace(array(&apos;/&apos;, &apos;\\&apos;), DIRECTORY_SEPARATOR, $path);
&#xA0; &#xA0; &#xA0; &#xA0; $parts = array_filter(explode(DIRECTORY_SEPARATOR, $path), &apos;strlen&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; $absolutes = array();
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($parts as $part) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;.&apos; == $part) continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;..&apos; == $part) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_pop($absolutes);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $absolutes[] = $part;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return implode(DIRECTORY_SEPARATOR, $absolutes);
&#xA0; &#xA0; }
?>
```


A test:


```
<?php
&#xA0; &#xA0; var_dump(get_absolute_path(&apos;this/is/../a/./test/.///is&apos;));
?>
```

Returns: string(14) &quot;this/a/test/is&quot; 

As you can so, it also produces Yoda-speak. :)

  

#





```
<?php

namespace MockingMagician\Organic\Helper;

class Path
{
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * There is a method that deal with Sven Arduwie proposal https://www.php.net/manual/en/function.realpath.php#84012
&#xA0; &#xA0;&#xA0; * And runeimp at gmail dot com proposal https://www.php.net/manual/en/function.realpath.php#112367
&#xA0; &#xA0;&#xA0; * @param string $path
&#xA0; &#xA0;&#xA0; * @return string
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public static function getAbsolute(string $path): string
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // Cleaning path regarding OS
&#xA0; &#xA0; &#xA0; &#xA0; $path = mb_ereg_replace(&apos;\\\\|/&apos;, DIRECTORY_SEPARATOR, $path, &apos;msr&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; // Check if path start with a separator (UNIX)
&#xA0; &#xA0; &#xA0; &#xA0; $startWithSeparator = $path[0] === DIRECTORY_SEPARATOR;
&#xA0; &#xA0; &#xA0; &#xA0; // Check if start with drive letter
&#xA0; &#xA0; &#xA0; &#xA0; preg_match(&apos;/^[a-z]:/&apos;, $path, $matches);
&#xA0; &#xA0; &#xA0; &#xA0; $startWithLetterDir = isset($matches[0]) ? $matches[0] : false;
&#xA0; &#xA0; &#xA0; &#xA0; // Get and filter empty sub paths
&#xA0; &#xA0; &#xA0; &#xA0; $subPaths = array_filter(explode(DIRECTORY_SEPARATOR, $path), &apos;mb_strlen&apos;);

&#xA0; &#xA0; &#xA0; &#xA0; $absolutes = [];
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($subPaths as $subPath) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;.&apos; === $subPath) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // if $startWithSeparator is false
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // and $startWithLetterDir
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // and (absolutes is empty or all previous values are ..)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // save absolute cause that&apos;s a relative and we can&apos;t deal with that and just forget that we want go up
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;..&apos; === $subPath
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &amp;&amp; !$startWithSeparator
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &amp;&amp; !$startWithLetterDir
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &amp;&amp; empty(array_filter($absolutes, function ($value) { return !(&apos;..&apos; === $value); }))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $absolutes[] = $subPath;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;..&apos; === $subPath) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_pop($absolutes);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $absolutes[] = $subPath;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (($startWithSeparator ? DIRECTORY_SEPARATOR : $startWithLetterDir) ?
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $startWithLetterDir.DIRECTORY_SEPARATOR : &apos;&apos;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ).implode(DIRECTORY_SEPARATOR, $absolutes);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Examples
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;/one/two/../two/./three/../../two&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; &#xA0; /one/two
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;../one/two/../two/./three/../../two&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; &#xA0; ../one/two
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;../.././../one/two/../two/./three/../../two&apos;);&#xA0; =&gt;&#xA0; &#xA0; ../../../one/two
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;../././../one/two/../two/./three/../../two&apos;);&#xA0;&#xA0; =&gt;&#xA0; &#xA0; ../../one/two
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;/../one/two/../two/./three/../../two&apos;);&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; =&gt;&#xA0; &#xA0; /one/two
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;/../../one/two/../two/./three/../../two&apos;);&#xA0; &#xA0; &#xA0; =&gt;&#xA0; &#xA0; /one/two
&#xA0; &#xA0;&#xA0; * echo Path::getAbsolute(&apos;c:\.\..\one\two\..\two\.\three\..\..\two&apos;);&#xA0; &#xA0;&#xA0; =&gt;&#xA0; &#xA0; c:/one/two
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; */
}


  

#



Needed a method to normalize a virtual path that could handle .. references that go beyond the initial folder reference. So I created the following.


```
<?php

function normalizePath($path)
{
&#xA0; &#xA0; $parts = array();// Array to build a new path from the good parts
&#xA0; &#xA0; $path = str_replace(&apos;\\&apos;, &apos;/&apos;, $path);// Replace backslashes with forwardslashes
&#xA0; &#xA0; $path = preg_replace(&apos;/\/+/&apos;, &apos;/&apos;, $path);// Combine multiple slashes into a single slash
&#xA0; &#xA0; $segments = explode(&apos;/&apos;, $path);// Collect path segments
&#xA0; &#xA0; $test = &apos;&apos;;// Initialize testing variable
&#xA0; &#xA0; foreach($segments as $segment)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if($segment != &apos;.&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $test = array_pop($parts);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(is_null($test))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $parts[] = $segment;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else if($segment == &apos;..&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($test == &apos;..&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $parts[] = $test;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($test == &apos;..&apos; || $test == &apos;&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $parts[] = $segment;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $parts[] = $test;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $parts[] = $segment;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; return implode(&apos;/&apos;, $parts);
}
?>
```


Will convert /path/to/test/.././..//..///..///../one/two/../three/filename
to ../../one/three/filename

  

#



Note: If you use this to check if a file exists, it&apos;s path will be cached, and returns true even if the file is removed (use file_exists instead).

  

#



Here&apos;s a function to canonicalize a URL containing relative paths. Ran into the problem when pulling links from a remote page.



```
<?php

function canonicalize($address)
{
&#xA0; &#xA0; $address = explode(&apos;/&apos;, $address);
&#xA0; &#xA0; $keys = array_keys($address, &apos;..&apos;);

&#xA0; &#xA0; foreach($keys AS $keypos =&gt; $key)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; array_splice($address, $key - ($keypos * 2 + 1), 2);
&#xA0; &#xA0; }

&#xA0; &#xA0; $address = implode(&apos;/&apos;, $address);
&#xA0; &#xA0; $address = str_replace(&apos;./&apos;, &apos;&apos;, $address);
}

$url = &apos;http://www.example.com/something/../else&apos;;
echo canonicalize($url); //http://www.example.com/else

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.realpath.php)

**[To root](/README.md)**