# glob





Since I feel this is rather vague and non-helpful, I thought I&apos;d make a post detailing the mechanics of the glob regex.



glob uses two special symbols that act like sort of a blend between a meta-character and a quantifier.&#xA0; These two characters are the * and ? 



The ? matches 1 of any character except a /

The * matches 0 or more of any character except a /



If it helps, think of the * as the pcre equivalent of .* and ? as the pcre equivalent of the dot (.)



Note: * and ? function independently from the previous character. For instance, if you do glob(&quot;a*.php&quot;) on the following list of files, all of the files starting with an &apos;a&apos; will be returned, but * itself would match:



a.php // * matches nothing

aa.php // * matches the second &apos;a&apos;

ab.php // * matches &apos;b&apos;

abc.php // * matches &apos;bc&apos;

b.php // * matches nothing, because the starting &apos;a&apos; fails

bc.php // * matches nothing, because the starting &apos;a&apos; fails

bcd.php // * matches nothing, because the starting &apos;a&apos; fails



It does not match just a.php and aa.php as a &apos;normal&apos; regex would, because it matches 0 or more of any character, not the character/class/group before it. 



Executing glob(&quot;a?.php&quot;) on the same list of files will only return aa.php and ab.php because as mentioned, the ? is the equivalent of pcre&apos;s dot, and is NOT the same as pcre&apos;s ?, which would match 0 or 1 of the previous character.



glob&apos;s regex also supports character classes and negative character classes, using the syntax [] and [^]. It will match any one character inside [] or match any one character that is not in [^].



With the same list above, executing 



glob(&quot;[ab]*.php) will return (all of them):

a.php&#xA0; // [ab] matches &apos;a&apos;, * matches nothing

aa.php // [ab] matches &apos;a&apos;, * matches 2nd &apos;a&apos;

ab.php // [ab] matches &apos;a&apos;, * matches &apos;b&apos;

abc.php // [ab] matches &apos;a&apos;, * matches &apos;bc&apos;

b.php // [ab] matches &apos;b&apos;, * matches nothing

bc.php // [ab] matches &apos;b&apos;, * matches &apos;c&apos;

bcd.php // [ab] matches &apos;b&apos;, * matches &apos;cd&apos;



glob(&quot;[ab].php&quot;) will return a.php and b.php



glob(&quot;[^a]*.php&quot;) will return:

b.php // [^a] matches &apos;b&apos;, * matches nothing

bc.php // [^a] matches &apos;b&apos;, * matches &apos;c&apos;

bcd.php // [^a] matches &apos;b&apos;, * matches &apos;cd&apos;



glob(&quot;[^ab]*.php&quot;) will return nothing because the character class will fail to match on the first character. 



You can also use ranges of characters inside the character class by having a starting and ending character with a hyphen in between.&#xA0; For example, [a-z] will match any letter between a and z, [0-9] will match any (one) number, etc.. 



glob also supports limited alternation with {n1, n2, etc..}.&#xA0; You have to specify GLOB_BRACE as the 2nd argument for glob in order for it to work.&#xA0; So for example, if you executed glob(&quot;{a,b,c}.php&quot;, GLOB_BRACE) on the following list of files:



a.php

b.php

c.php



all 3 of them would return.&#xA0; Note: using alternation with single characters like that is the same thing as just doing glob(&quot;[abc].php&quot;).&#xA0; A more interesting example would be glob(&quot;te{xt,nse}.php&quot;, GLOB_BRACE) on:



tent.php

text.php

test.php

tense.php



text.php and tense.php would be returned from that glob.



glob&apos;s regex does not offer any kind of quantification of a specified character or character class or alternation.&#xA0; For instance, if you have the following files:



a.php

aa.php

aaa.php

ab.php

abc.php

b.php

bc.php



with pcre regex you can do ~^a+\.php$~ to return 



a.php

aa.php

aaa.php



This is not possible with glob.&#xA0; If you are trying to do something like this, you can first narrow it down with glob, and then get exact matches with a full flavored regex engine.&#xA0; For example, if you wanted all of the php files in the previous list that only have one or more &apos;a&apos; in it, you can do this:





```
<?php

&#xA0;&#xA0; $list = glob(&quot;a*.php&quot;);

&#xA0;&#xA0; foreach ($list as $l) {

&#xA0; &#xA0; &#xA0; if (preg_match(&quot;~^a+\.php$~&quot;,$file))

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $files[] = $l;

&#xA0;&#xA0; }

?>
```




glob also does not support lookbehinds, lookaheads, atomic groupings, capturing, or any of the &apos;higher level&apos; regex functions.



glob does not support &apos;shortkey&apos; meta-characters like \w or \d.

  

#



Those of you with PHP 5 don&apos;t have to come up with these wild functions to scan a directory recursively: the SPL can do it.



```
<?php

$dir_iterator = new RecursiveDirectoryIterator(&quot;/path&quot;);
$iterator = new RecursiveIteratorIterator($dir_iterator, RecursiveIteratorIterator::SELF_FIRST);
// could use CHILD_FIRST if you so wish

foreach ($iterator as $file) {
&#xA0; &#xA0; echo $file, &quot;\n&quot;;
}

?>
```


Not to mention the fact that $file will be an SplFileInfo class, so you can do powerful stuff really easily:



```
<?php

$size = 0;
foreach ($iterator as $file) {
&#xA0; &#xA0; if ($file-&gt;isFile()) {
&#xA0; &#xA0; &#xA0; &#xA0; echo substr($file-&gt;getPathname(), 27) . &quot;: &quot; . $file-&gt;getSize() . &quot; B; modified &quot; . date(&quot;Y-m-d&quot;, $file-&gt;getMTime()) . &quot;\n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $size += $file-&gt;getSize();
&#xA0; &#xA0; }
}

echo &quot;\nTotal file size: &quot;, $size, &quot; bytes\n&quot;;

?>
```


\Luna\luna.msstyles: 4190352 B; modified 2008-04-13
\Luna\Shell\Homestead\shellstyle.dll: 362496 B; modified 2006-02-28
\Luna\Shell\Metallic\shellstyle.dll: 362496 B; modified 2006-02-28
\Luna\Shell\NormalColor\shellstyle.dll: 361472 B; modified 2006-02-28
\Luna.theme: 1222 B; modified 2006-02-28
\Windows Classic.theme: 3025 B; modified 2006-02-28

Total file size: 5281063 bytes

  

#



Please note that glob(&apos;*&apos;) ignores all &apos;hidden&apos; files by default. This means it does not return files that start with a dot (e.g. &quot;.file&quot;).

If you want to match those files too, you can use &quot;{,.}*&quot; as the pattern with the GLOB_BRACE flag.





```
<?php

// Search for all files that match .* or *

$files = glob(&apos;{,.}*&apos;, GLOB_BRACE);

?>
```




Note: This also returns the directory special entries . and ..

  

#



glob is case sensitive, even on Windows systems.

It does support character classes though, so a case insensitive version of


```
<?php glob(&apos;my/dir/*.csv&apos;) ?>
```


could be written as


```
<?php glob(&apos;my/dir/*.[cC][sS][vV]&apos;) ?>
```



  

#



glob() isn&apos;t limited to one directory:



```
<?php
$results=glob(&quot;{includes/*.php,core/*.php}&quot;,GLOB_BRACE);
echo &apos;&lt;pre&gt;&apos;,print_r($results,true),&apos;&lt;/pre&gt;&apos;;
?>
```


Just be careful when using GLOB_BRACE regarding spaces around the comma:
{includes/*.php,core/*.php} works as expected, but
{includes/*.php, core/*.php} with a leading space, will only match the former as expected but not the latter
unless you have a directory named &quot; core&quot; on your machine with a leading space.
PHP can create such directories quite easily like so:
mkdir(&quot; core&quot;);

  

#



Note that in case you are using braces with glob you might retrieve duplicated entries for files that matche more than one item :



```
<?php

$a = glob(&apos;/path/*{foo,bar}.dat&apos;,GLOB_BRACE);
print_r($a);

?>
```


Result : 
Array
(
&#xA0; &#xA0; [0] =&gt; /path/file_foo.dat
&#xA0; &#xA0; [1] =&gt; /path/file_foobar.dat
&#xA0; &#xA0; [2] =&gt; /path/file_foobar.dat
)

  

#



Don&apos;t use glob() if you try to list files in a directory where very much files are stored (&gt;100.000). You get an &quot;Allowed memory size of XYZ bytes exhausted ...&quot; error.
You may try to increase the memory_limit variable in php.ini. Mine has 128MB set and the script will still reach this limit while glob()ing over 500.000 files.

The more stable way is to use readdir() on very large numbers of files:


```
<?php
// code snippet
if ($handle = opendir($path)) {
&#xA0; &#xA0; while (false !== ($file = readdir($handle))) {
&#xA0; &#xA0; &#xA0; &#xA0; // do something with the file
&#xA0; &#xA0; &#xA0; &#xA0; // note that &apos;.&apos; and &apos;..&apos; is returned even
&#xA0; &#xA0; }
&#xA0; &#xA0; closedir($handle);
}
?>
```



  

#





```
<?php

if ( ! function_exists(&apos;glob_recursive&apos;))
{
&#xA0; &#xA0; // Does not support flag GLOB_BRACE
&#xA0; &#xA0; 
&#xA0; &#xA0; function glob_recursive($pattern, $flags = 0)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $files = glob($pattern, $flags);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; foreach (glob(dirname($pattern).&apos;/*&apos;, GLOB_ONLYDIR|GLOB_NOSORT) as $dir)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $files = array_merge($files, glob_recursive($dir.&apos;/&apos;.basename($pattern), $flags));
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; return $files;
&#xA0; &#xA0; }
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.glob.php)

**[To root](/README.md)**