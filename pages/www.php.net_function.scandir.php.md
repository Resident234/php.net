# scandir





Easy way to get rid of the dots that scandir() picks up in Linux environments:



```
<?php
$directory = &apos;/path/to/my/directory&apos;;
$scanned_directory = array_diff(scandir($directory), array(&apos;..&apos;, &apos;.&apos;));
?>
```



  

#



Here is my 2 cents. I wanted to create an array of my directory structure recursively. I wanted to easely access data in a certain directory using foreach. I came up with the following:





```
<?php

function dirToArray($dir) {

&#xA0;&#xA0; 

&#xA0;&#xA0; $result = array();



&#xA0;&#xA0; $cdir = scandir($dir);

&#xA0;&#xA0; foreach ($cdir as $key =&gt; $value)

&#xA0;&#xA0; {

&#xA0; &#xA0; &#xA0; if (!in_array($value,array(&quot;.&quot;,&quot;..&quot;)))

&#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (is_dir($dir . DIRECTORY_SEPARATOR . $value))

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result[$value] = dirToArray($dir . DIRECTORY_SEPARATOR . $value);

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; else

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result[] = $value;

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } 

&#xA0; &#xA0; &#xA0; }

&#xA0;&#xA0; }

&#xA0;&#xA0; 

&#xA0;&#xA0; return $result;

}

?>
```




Output

Array

(

&#xA0;&#xA0; [subdir1] =&gt; Array

&#xA0;&#xA0; (

&#xA0; &#xA0; &#xA0; [0] =&gt; file1.txt

&#xA0; &#xA0; &#xA0; [subsubdir] =&gt; Array

&#xA0; &#xA0; &#xA0; (

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; [0] =&gt; file2.txt

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; [1] =&gt; file3.txt

&#xA0; &#xA0; &#xA0; )

&#xA0;&#xA0; )

&#xA0;&#xA0; [subdir2] =&gt; Array

&#xA0;&#xA0; (

&#xA0; &#xA0; [0] =&gt; file4.txt

&#xA0;&#xA0; }

)

  

#



Someone wrote that array_slice could be used to quickly remove directory entries &quot;.&quot; and &quot;..&quot;. However, &quot;-&quot; is a valid entry that would come before those, so array_slice would remove the wrong entries.

  

#



Fastest way to get a list of files without dots.


```
<?php
$files = array_slice(scandir(&apos;/path/to/directory/&apos;), 2);


  

#



Needed something that could return the contents of single or multiple directories, recursively or non-recursively,
for all files or specified file extensions that would be
accessible easily from any scope or script.

And I wanted to allow overloading cause sometimes I&apos;m too lazy to pass all params.


```
<?php
class scanDir {
&#xA0; &#xA0; static private $directories, $files, $ext_filter, $recursive;

// ----------------------------------------------------------------------------------------------
&#xA0; &#xA0; // scan(dirpath::string|array, extensions::string|array, recursive::true|false)
&#xA0; &#xA0; static public function scan(){
&#xA0; &#xA0; &#xA0; &#xA0; // Initialize defaults
&#xA0; &#xA0; &#xA0; &#xA0; self::$recursive = false;
&#xA0; &#xA0; &#xA0; &#xA0; self::$directories = array();
&#xA0; &#xA0; &#xA0; &#xA0; self::$files = array();
&#xA0; &#xA0; &#xA0; &#xA0; self::$ext_filter = false;

&#xA0; &#xA0; &#xA0; &#xA0; // Check we have minimum parameters
&#xA0; &#xA0; &#xA0; &#xA0; if(!$args = func_get_args()){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die(&quot;Must provide a path string or array of path strings&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if(gettype($args[0]) != &quot;string&quot; &amp;&amp; gettype($args[0]) != &quot;array&quot;){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die(&quot;Must provide a path string or array of path strings&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Check if recursive scan | default action: no sub-directories
&#xA0; &#xA0; &#xA0; &#xA0; if(isset($args[2]) &amp;&amp; $args[2] == true){self::$recursive = true;}

&#xA0; &#xA0; &#xA0; &#xA0; // Was a filter on file extensions included? | default action: return all file types
&#xA0; &#xA0; &#xA0; &#xA0; if(isset($args[1])){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(gettype($args[1]) == &quot;array&quot;){self::$ext_filter = array_map(&apos;strtolower&apos;, $args[1]);}
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(gettype($args[1]) == &quot;string&quot;){self::$ext_filter[] = strtolower($args[1]);}
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Grab path(s)
&#xA0; &#xA0; &#xA0; &#xA0; self::verifyPaths($args[0]);
&#xA0; &#xA0; &#xA0; &#xA0; return self::$files;
&#xA0; &#xA0; }

&#xA0; &#xA0; static private function verifyPaths($paths){
&#xA0; &#xA0; &#xA0; &#xA0; $path_errors = array();
&#xA0; &#xA0; &#xA0; &#xA0; if(gettype($paths) == &quot;string&quot;){$paths = array($paths);}

&#xA0; &#xA0; &#xA0; &#xA0; foreach($paths as $path){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(is_dir($path)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$directories[] = $path;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $dirContents = self::find_contents($path);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $path_errors[] = $path;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; if($path_errors){echo &quot;The following directories do not exists&lt;br /&gt;&quot;;die(var_dump($path_errors));}
&#xA0; &#xA0; }

&#xA0; &#xA0; // This is how we scan directories
&#xA0; &#xA0; static private function find_contents($dir){
&#xA0; &#xA0; &#xA0; &#xA0; $result = array();
&#xA0; &#xA0; &#xA0; &#xA0; $root = scandir($dir);
&#xA0; &#xA0; &#xA0; &#xA0; foreach($root as $value){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($value === &apos;.&apos; || $value === &apos;..&apos;) {continue;}
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(is_file($dir.DIRECTORY_SEPARATOR.$value)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!self::$ext_filter || in_array(strtolower(pathinfo($dir.DIRECTORY_SEPARATOR.$value, PATHINFO_EXTENSION)), self::$ext_filter)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$files[] = $result[] = $dir.DIRECTORY_SEPARATOR.$value;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(self::$recursive){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(self::find_contents($dir.DIRECTORY_SEPARATOR.$value) as $value) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$files[] = $result[] = $value;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; // Return required for recursive search
&#xA0; &#xA0; &#xA0; &#xA0; return $result;
&#xA0; &#xA0; }
}
?>
```


Usage:
scanDir::scan(path(s):string|array, [file_extensions:string|array], [subfolders?:true|false]);


```
<?php
//Scan a single directory for all files, no sub-directories
$files = scanDir::scan(&apos;D:\Websites\temp&apos;);

//Scan multiple directories for all files, no sub-dirs
$dirs = array(
&#xA0; &#xA0; &apos;D:\folder&apos;;
&#xA0; &#xA0; &apos;D:\folder2&apos;;
&#xA0; &#xA0; &apos;C:\Other&apos;;
);
$files = scanDir::scan($dirs);

// Scan multiple directories for files with provided file extension,
// no sub-dirs
$files = scanDir::scan($dirs, &quot;jpg&quot;);
//or with an array of extensions
$file_ext = array(
&#xA0; &#xA0; &quot;jpg&quot;,
&#xA0; &#xA0; &quot;bmp&quot;,
&#xA0; &#xA0; &quot;png&quot;
);
$files = scanDir::scan($dirs, $file_ext);

// Scan multiple directories for files with any extension,
// include files in recursive sub-folders
$files = scanDir::scan($dirs, false, true);

// Multiple dirs, with specified extensions, include sub-dir files
$files = scanDir::scan($dirs, $file_ext, true);
?>
```



  

#



I needed to find a way to get the full path of all files in the directory and all subdirectories of a directory.

Here&apos;s my solution: Recursive functions!





```
<?php

function find_all_files($dir)

{

&#xA0; &#xA0; $root = scandir($dir);

&#xA0; &#xA0; foreach($root as $value)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; if($value === &apos;.&apos; || $value === &apos;..&apos;) {continue;}

&#xA0; &#xA0; &#xA0; &#xA0; if(is_file(&quot;$dir/$value&quot;)) {$result[]=&quot;$dir/$value&quot;;continue;}

&#xA0; &#xA0; &#xA0; &#xA0; foreach(find_all_files(&quot;$dir/$value&quot;) as $value)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result[]=$value;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; return $result;

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.scandir.php)

**[To root](/README.md)**