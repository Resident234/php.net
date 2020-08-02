# glob



Since I feel this is rather vague and non-helpful, I thought I&apos;d make a post detailing the mechanics of the glob regex.<br><br>glob uses two special symbols that act like sort of a blend between a meta-character and a quantifier.  These two characters are the * and ? <br><br>The ? matches 1 of any character except a /<br>The * matches 0 or more of any character except a /<br><br>If it helps, think of the * as the pcre equivalent of .* and ? as the pcre equivalent of the dot (.)<br><br>Note: * and ? function independently from the previous character. For instance, if you do glob("a*.php") on the following list of files, all of the files starting with an &apos;a&apos; will be returned, but * itself would match:<br><br>a.php // * matches nothing<br>aa.php // * matches the second &apos;a&apos;<br>ab.php // * matches &apos;b&apos;<br>abc.php // * matches &apos;bc&apos;<br>b.php // * matches nothing, because the starting &apos;a&apos; fails<br>bc.php // * matches nothing, because the starting &apos;a&apos; fails<br>bcd.php // * matches nothing, because the starting &apos;a&apos; fails<br><br>It does not match just a.php and aa.php as a &apos;normal&apos; regex would, because it matches 0 or more of any character, not the character/class/group before it. <br><br>Executing glob("a?.php") on the same list of files will only return aa.php and ab.php because as mentioned, the ? is the equivalent of pcre&apos;s dot, and is NOT the same as pcre&apos;s ?, which would match 0 or 1 of the previous character.<br><br>glob&apos;s regex also supports character classes and negative character classes, using the syntax [] and [^]. It will match any one character inside [] or match any one character that is not in [^].<br><br>With the same list above, executing <br><br>glob("[ab]*.php) will return (all of them):<br>a.php  // [ab] matches &apos;a&apos;, * matches nothing<br>aa.php // [ab] matches &apos;a&apos;, * matches 2nd &apos;a&apos;<br>ab.php // [ab] matches &apos;a&apos;, * matches &apos;b&apos;<br>abc.php // [ab] matches &apos;a&apos;, * matches &apos;bc&apos;<br>b.php // [ab] matches &apos;b&apos;, * matches nothing<br>bc.php // [ab] matches &apos;b&apos;, * matches &apos;c&apos;<br>bcd.php // [ab] matches &apos;b&apos;, * matches &apos;cd&apos;<br><br>glob("[ab].php") will return a.php and b.php<br><br>glob("[^a]*.php") will return:<br>b.php // [^a] matches &apos;b&apos;, * matches nothing<br>bc.php // [^a] matches &apos;b&apos;, * matches &apos;c&apos;<br>bcd.php // [^a] matches &apos;b&apos;, * matches &apos;cd&apos;<br><br>glob("[^ab]*.php") will return nothing because the character class will fail to match on the first character. <br><br>You can also use ranges of characters inside the character class by having a starting and ending character with a hyphen in between.  For example, [a-z] will match any letter between a and z, [0-9] will match any (one) number, etc.. <br><br>glob also supports limited alternation with {n1, n2, etc..}.  You have to specify GLOB_BRACE as the 2nd argument for glob in order for it to work.  So for example, if you executed glob("{a,b,c}.php", GLOB_BRACE) on the following list of files:<br><br>a.php<br>b.php<br>c.php<br><br>all 3 of them would return.  Note: using alternation with single characters like that is the same thing as just doing glob("[abc].php").  A more interesting example would be glob("te{xt,nse}.php", GLOB_BRACE) on:<br><br>tent.php<br>text.php<br>test.php<br>tense.php<br><br>text.php and tense.php would be returned from that glob.<br><br>glob&apos;s regex does not offer any kind of quantification of a specified character or character class or alternation.  For instance, if you have the following files:<br><br>a.php<br>aa.php<br>aaa.php<br>ab.php<br>abc.php<br>b.php<br>bc.php<br><br>with pcre regex you can do ~^a+\.php$~ to return <br><br>a.php<br>aa.php<br>aaa.php<br><br>This is not possible with glob.  If you are trying to do something like this, you can first narrow it down with glob, and then get exact matches with a full flavored regex engine.  For example, if you wanted all of the php files in the previous list that only have one or more &apos;a&apos; in it, you can do this:<br><br>

```
<?php
   $list = glob("a*.php");
   foreach ($list as $l) {
      if (preg_match("~^a+\.php$~",$file))
         $files[] = $l;
   }
?>
```
<br><br>glob also does not support lookbehinds, lookaheads, atomic groupings, capturing, or any of the &apos;higher level&apos; regex functions.<br><br>glob does not support &apos;shortkey&apos; meta-characters like \w or \d.  

---

Those of you with PHP 5 don&apos;t have to come up with these wild functions to scan a directory recursively: the SPL can do it.<br><br>

```
<?php

$dir_iterator = new RecursiveDirectoryIterator("/path");
$iterator = new RecursiveIteratorIterator($dir_iterator, RecursiveIteratorIterator::SELF_FIRST);
// could use CHILD_FIRST if you so wish

foreach ($iterator as $file) {
    echo $file, "\n";
}

?>
```


Not to mention the fact that $file will be an SplFileInfo class, so you can do powerful stuff really easily:



```
<?php

$size = 0;
foreach ($iterator as $file) {
    if ($file->isFile()) {
        echo substr($file->getPathname(), 27) . ": " . $file->getSize() . " B; modified " . date("Y-m-d", $file->getMTime()) . "\n";
        $size += $file->getSize();
    }
}

echo "\nTotal file size: ", $size, " bytes\n";

?>
```
<br><br>\Luna\luna.msstyles: 4190352 B; modified 2008-04-13<br>\Luna\Shell\Homestead\shellstyle.dll: 362496 B; modified 2006-02-28<br>\Luna\Shell\Metallic\shellstyle.dll: 362496 B; modified 2006-02-28<br>\Luna\Shell\NormalColor\shellstyle.dll: 361472 B; modified 2006-02-28<br>\Luna.theme: 1222 B; modified 2006-02-28<br>\Windows Classic.theme: 3025 B; modified 2006-02-28<br><br>Total file size: 5281063 bytes  

---

Please note that glob(&apos;*&apos;) ignores all &apos;hidden&apos; files by default. This means it does not return files that start with a dot (e.g. ".file").<br>If you want to match those files too, you can use "{,.}*" as the pattern with the GLOB_BRACE flag.<br><br>

```
<?php
// Search for all files that match .* or *
$files = glob('{,.}*', GLOB_BRACE);
?>
```
<br><br>Note: This also returns the directory special entries . and ..  

---

glob is case sensitive, even on Windows systems.<br><br>It does support character classes though, so a case insensitive version of<br>

```
<?php glob('my/dir/*.csv') ?>
```


could be written as


```
<?php glob('my/dir/*.[cC][sS][vV]') ?>
```
  

---

glob() isn&apos;t limited to one directory:<br><br>

```
<?php
$results=glob("{includes/*.php,core/*.php}",GLOB_BRACE);
echo '',print_r($results,true),'';
?>
```
<br><br>Just be careful when using GLOB_BRACE regarding spaces around the comma:<br>{includes/*.php,core/*.php} works as expected, but<br>{includes/*.php, core/*.php} with a leading space, will only match the former as expected but not the latter<br>unless you have a directory named " core" on your machine with a leading space.<br>PHP can create such directories quite easily like so:<br>mkdir(" core");  

---

Note that in case you are using braces with glob you might retrieve duplicated entries for files that matche more than one item :<br><br>

```
<?php

$a = glob('/path/*{foo,bar}.dat',GLOB_BRACE);
print_r($a);

?>
```
<br><br>Result : <br>Array<br>(<br>    [0] =&gt; /path/file_foo.dat<br>    [1] =&gt; /path/file_foobar.dat<br>    [2] =&gt; /path/file_foobar.dat<br>)  

---

Don&apos;t use glob() if you try to list files in a directory where very much files are stored (&gt;100.000). You get an "Allowed memory size of XYZ bytes exhausted ..." error.<br>You may try to increase the memory_limit variable in php.ini. Mine has 128MB set and the script will still reach this limit while glob()ing over 500.000 files.<br><br>The more stable way is to use readdir() on very large numbers of files:<br>

```
<?php
// code snippet
if ($handle = opendir($path)) {
    while (false !== ($file = readdir($handle))) {
        // do something with the file
        // note that '.' and '..' is returned even
    }
    closedir($handle);
}
?>
```
  

---



```
<?php

if ( ! function_exists('glob_recursive'))
{
    // Does not support flag GLOB_BRACE
    
    function glob_recursive($pattern, $flags = 0)
    {
        $files = glob($pattern, $flags);
        
        foreach (glob(dirname($pattern).'/*', GLOB_ONLYDIR|GLOB_NOSORT) as $dir)
        {
            $files = array_merge($files, glob_recursive($dir.'/'.basename($pattern), $flags));
        }
        
        return $files;
    }
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.glob.php)

**[To root](/README.md)**