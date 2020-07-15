# realpath



Because realpath() does not work on files that do not<br>exist, I wrote a function that does.<br>It replaces (consecutive) occurences of / and \\ with<br>whatever is in DIRECTORY_SEPARATOR, and processes /. and /.. fine.<br>Paths returned by get_absolute_path() contain no<br>(back)slash at position 0 (beginning of the string) or<br>position -1 (ending)<br>

```
<?php
    function get_absolute_path($path) {
        $path = str_replace(array('/', '\\'), DIRECTORY_SEPARATOR, $path);
        $parts = array_filter(explode(DIRECTORY_SEPARATOR, $path), 'strlen');
        $absolutes = array();
        foreach ($parts as $part) {
            if ('.' == $part) continue;
            if ('..' == $part) {
                array_pop($absolutes);
            } else {
                $absolutes[] = $part;
            }
        }
        return implode(DIRECTORY_SEPARATOR, $absolutes);
    }
?>
```


A test:


```
<?php
    var_dump(get_absolute_path('this/is/../a/./test/.///is'));
?>
```
<br>Returns: string(14) "this/a/test/is" <br><br>As you can so, it also produces Yoda-speak. :)  

#



```
<?php

namespace MockingMagician\Organic\Helper;

class Path
{
    /**
     * There is a method that deal with Sven Arduwie proposal https://www.php.net/manual/en/function.realpath.php#84012
     * And runeimp at gmail dot com proposal https://www.php.net/manual/en/function.realpath.php#112367
     * @param string $path
     * @return string
     */
    public static function getAbsolute(string $path): string
    {
        // Cleaning path regarding OS
        $path = mb_ereg_replace('\\\\|/', DIRECTORY_SEPARATOR, $path, 'msr');
        // Check if path start with a separator (UNIX)
        $startWithSeparator = $path[0] === DIRECTORY_SEPARATOR;
        // Check if start with drive letter
        preg_match('/^[a-z]:/', $path, $matches);
        $startWithLetterDir = isset($matches[0]) ? $matches[0] : false;
        // Get and filter empty sub paths
        $subPaths = array_filter(explode(DIRECTORY_SEPARATOR, $path), 'mb_strlen');

        $absolutes = [];
        foreach ($subPaths as $subPath) {
            if ('.' === $subPath) {
                continue;
            }
            // if $startWithSeparator is false
            // and $startWithLetterDir
            // and (absolutes is empty or all previous values are ..)
            // save absolute cause that's a relative and we can't deal with that and just forget that we want go up
            if ('..' === $subPath
                &amp;&amp; !$startWithSeparator
                &amp;&amp; !$startWithLetterDir
                &amp;&amp; empty(array_filter($absolutes, function ($value) { return !('..' === $value); }))
            ) {
                $absolutes[] = $subPath;
                continue;
            }
            if ('..' === $subPath) {
                array_pop($absolutes);
                continue;
            }
            $absolutes[] = $subPath;
        }

        return
            (($startWithSeparator ? DIRECTORY_SEPARATOR : $startWithLetterDir) ?
                $startWithLetterDir.DIRECTORY_SEPARATOR : ''
            ).implode(DIRECTORY_SEPARATOR, $absolutes);
    }

    /**
     * Examples
     *
     * echo Path::getAbsolute('/one/two/../two/./three/../../two');            =>    /one/two
     * echo Path::getAbsolute('../one/two/../two/./three/../../two');          =>    ../one/two
     * echo Path::getAbsolute('../.././../one/two/../two/./three/../../two');  =>    ../../../one/two
     * echo Path::getAbsolute('../././../one/two/../two/./three/../../two');   =>    ../../one/two
     * echo Path::getAbsolute('/../one/two/../two/./three/../../two');         =>    /one/two
     * echo Path::getAbsolute('/../../one/two/../two/./three/../../two');      =>    /one/two
     * echo Path::getAbsolute('c:\.\..\one\two\..\two\.\three\..\..\two');     =>    c:/one/two
     *
     */
}?>
```
  

#

Needed a method to normalize a virtual path that could handle .. references that go beyond the initial folder reference. So I created the following.<br>

```
<?php

function normalizePath($path)
{
    $parts = array();// Array to build a new path from the good parts
    $path = str_replace('\\', '/', $path);// Replace backslashes with forwardslashes
    $path = preg_replace('/\/+/', '/', $path);// Combine multiple slashes into a single slash
    $segments = explode('/', $path);// Collect path segments
    $test = '';// Initialize testing variable
    foreach($segments as $segment)
    {
        if($segment != '.')
        {
            $test = array_pop($parts);
            if(is_null($test))
                $parts[] = $segment;
            else if($segment == '..')
            {
                if($test == '..')
                    $parts[] = $test;

                if($test == '..' || $test == '')
                    $parts[] = $segment;
            }
            else
            {
                $parts[] = $test;
                $parts[] = $segment;
            }
        }
    }
    return implode('/', $parts);
}
?>
```
<br><br>Will convert /path/to/test/.././..//..///..///../one/two/../three/filename<br>to ../../one/three/filename  

#

Note: If you use this to check if a file exists, it&apos;s path will be cached, and returns true even if the file is removed (use file_exists instead).  

#

Here&apos;s a function to canonicalize a URL containing relative paths. Ran into the problem when pulling links from a remote page.<br><br>

```
<?php

function canonicalize($address)
{
    $address = explode('/', $address);
    $keys = array_keys($address, '..');

    foreach($keys AS $keypos => $key)
    {
        array_splice($address, $key - ($keypos * 2 + 1), 2);
    }

    $address = implode('/', $address);
    $address = str_replace('./', '', $address);
}

$url = 'http://www.example.com/something/../else';
echo canonicalize($url); //http://www.example.com/else

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.realpath.php)

**[To root](/README.md)**