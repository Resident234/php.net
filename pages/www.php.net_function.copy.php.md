# copy



Having spent hours tacking down a copy() error: Permission denied , (and duly worrying about chmod on winXP) , its worth pointing out that the &apos;destination&apos; needs to contain the actual file name ! --- NOT just the path to the folder you wish to copy into.......<br>DOH !<br>hope this saves somebody hours of fruitless debugging  

#

It take me a long time to find out what the problem is when i&apos;ve got an error on copy(). It DOESN&apos;T create any directories. It only copies to existing path. So create directories before. Hope i&apos;ll help,  

#

Don&apos;t forget; you can use copy on remote files, rather than doing messy fopen stuff.  e.g.<br><br>

```
<?php
if(!@copy(&apos;http://someserver.com/somefile.zip&apos;,&apos;./somefile.zip&apos;))
{
    $errors= error_get_last();
    echo "COPY ERROR: ".$errors[&apos;type&apos;];
    echo "&lt;br /&gt;\n".$errors[&apos;message&apos;];
} else {
    echo "File copied from remote!";
}
?>
```
  

#

A nice simple trick if you need to make sure the folder exists first:<br><br>

```
<?php

$srcfile=&apos;C:\File\Whatever\Path\Joe.txt&apos;;
$dstfile=&apos;G:\Shared\Reports\Joe.txt&apos;;
mkdir(dirname($dstfile), 0777, true);
copy($srcfile, $dstfile);

?>
```
<br><br>That simple.  

#

Here is a simple script that I use for removing and copying non-empty directories. Very useful when you are not sure what is the type of a file.<br><br>I am using these for managing folders and zip archives for my website plugins.<br><br>

```
<?php

// removes files and non-empty directories
function rrmdir($dir) {
  if (is_dir($dir)) {
    $files = scandir($dir);
    foreach ($files as $file)
    if ($file != "." &amp;&amp; $file != "..") rrmdir("$dir/$file");
    rmdir($dir);
  }
  else if (file_exists($dir)) unlink($dir);
} 

// copies files and non-empty directories
function rcopy($src, $dst) {
  if (file_exists($dst)) rrmdir($dst);
  if (is_dir($src)) {
    mkdir($dst);
    $files = scandir($src);
    foreach ($files as $file)
    if ($file != "." &amp;&amp; $file != "..") rcopy("$src/$file", "$dst/$file"); 
  }
  else if (file_exists($src)) copy($src, $dst);
}
?>
```
<br><br>Cheers!  

#

Here&apos;s a simple recursive function to copy entire directories<br><br>Note to do your own check to make sure the directory exists that you first call it on.<br><br>

```
<?php
function recurse_copy($src,$dst) {
    $dir = opendir($src);
    @mkdir($dst);
    while(false !== ( $file = readdir($dir)) ) {
        if (( $file != &apos;.&apos; ) &amp;&amp; ( $file != &apos;..&apos; )) {
            if ( is_dir($src . &apos;/&apos; . $file) ) {
                recurse_copy($src . &apos;/&apos; . $file,$dst . &apos;/&apos; . $file);
            }
            else { 
                copy($src . &apos;/&apos; . $file,$dst . &apos;/&apos; . $file);
            }
        }
    }
    closedir($dir);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.copy.php)

**[To root](/README.md)**