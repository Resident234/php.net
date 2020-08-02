# scandir



Easy way to get rid of the dots that scandir() picks up in Linux environments:<br><br>

```
<?php
$directory = '/path/to/my/directory';
$scanned_directory = array_diff(scandir($directory), array('..', '.'));
?>
```
  

---

Here is my 2 cents. I wanted to create an array of my directory structure recursively. I wanted to easely access data in a certain directory using foreach. I came up with the following:<br><br>

```
<?php
function dirToArray($dir) {
   
   $result = array();

   $cdir = scandir($dir);
   foreach ($cdir as $key => $value)
   {
      if (!in_array($value,array(".","..")))
      {
         if (is_dir($dir . DIRECTORY_SEPARATOR . $value))
         {
            $result[$value] = dirToArray($dir . DIRECTORY_SEPARATOR . $value);
         }
         else
         {
            $result[] = $value;
         } 
      }
   }
   
   return $result;
}
?>
```
<br><br>Output<br>Array<br>(<br>   [subdir1] =&gt; Array<br>   (<br>      [0] =&gt; file1.txt<br>      [subsubdir] =&gt; Array<br>      (<br>         [0] =&gt; file2.txt<br>         [1] =&gt; file3.txt<br>      )<br>   )<br>   [subdir2] =&gt; Array<br>   (<br>    [0] =&gt; file4.txt<br>   }<br>)  

---

Someone wrote that array_slice could be used to quickly remove directory entries "." and "..". However, "-" is a valid entry that would come before those, so array_slice would remove the wrong entries.  

---

Fastest way to get a list of files without dots.<br>

```
<?php
$files = array_slice(scandir('/path/to/directory/'), 2);?>
```
  

---

Needed something that could return the contents of single or multiple directories, recursively or non-recursively,<br>for all files or specified file extensions that would be<br>accessible easily from any scope or script.<br><br>And I wanted to allow overloading cause sometimes I&apos;m too lazy to pass all params.<br>

```
<?php
class scanDir {
    static private $directories, $files, $ext_filter, $recursive;

// ----------------------------------------------------------------------------------------------
    // scan(dirpath::string|array, extensions::string|array, recursive::true|false)
    static public function scan(){
        // Initialize defaults
        self::$recursive = false;
        self::$directories = array();
        self::$files = array();
        self::$ext_filter = false;

        // Check we have minimum parameters
        if(!$args = func_get_args()){
            die("Must provide a path string or array of path strings");
        }
        if(gettype($args[0]) != "string" &amp;&amp; gettype($args[0]) != "array"){
            die("Must provide a path string or array of path strings");
        }

        // Check if recursive scan | default action: no sub-directories
        if(isset($args[2]) &amp;&amp; $args[2] == true){self::$recursive = true;}

        // Was a filter on file extensions included? | default action: return all file types
        if(isset($args[1])){
            if(gettype($args[1]) == "array"){self::$ext_filter = array_map('strtolower', $args[1]);}
            else
            if(gettype($args[1]) == "string"){self::$ext_filter[] = strtolower($args[1]);}
        }

        // Grab path(s)
        self::verifyPaths($args[0]);
        return self::$files;
    }

    static private function verifyPaths($paths){
        $path_errors = array();
        if(gettype($paths) == "string"){$paths = array($paths);}

        foreach($paths as $path){
            if(is_dir($path)){
                self::$directories[] = $path;
                $dirContents = self::find_contents($path);
            } else {
                $path_errors[] = $path;
            }
        }

        if($path_errors){echo "The following directories do not exists<br />";die(var_dump($path_errors));}
    }

    // This is how we scan directories
    static private function find_contents($dir){
        $result = array();
        $root = scandir($dir);
        foreach($root as $value){
            if($value === '.' || $value === '..') {continue;}
            if(is_file($dir.DIRECTORY_SEPARATOR.$value)){
                if(!self::$ext_filter || in_array(strtolower(pathinfo($dir.DIRECTORY_SEPARATOR.$value, PATHINFO_EXTENSION)), self::$ext_filter)){
                    self::$files[] = $result[] = $dir.DIRECTORY_SEPARATOR.$value;
                }
                continue;
            }
            if(self::$recursive){
                foreach(self::find_contents($dir.DIRECTORY_SEPARATOR.$value) as $value) {
                    self::$files[] = $result[] = $value;
                }
            }
        }
        // Return required for recursive search
        return $result;
    }
}
?>
```


Usage:
scanDir::scan(path(s):string|array, [file_extensions:string|array], [subfolders?:true|false]);


```
<?php
//Scan a single directory for all files, no sub-directories
$files = scanDir::scan('D:\Websites\temp');

//Scan multiple directories for all files, no sub-dirs
$dirs = array(
    'D:\folder';
    'D:\folder2';
    'C:\Other';
);
$files = scanDir::scan($dirs);

// Scan multiple directories for files with provided file extension,
// no sub-dirs
$files = scanDir::scan($dirs, "jpg");
//or with an array of extensions
$file_ext = array(
    "jpg",
    "bmp",
    "png"
);
$files = scanDir::scan($dirs, $file_ext);

// Scan multiple directories for files with any extension,
// include files in recursive sub-folders
$files = scanDir::scan($dirs, false, true);

// Multiple dirs, with specified extensions, include sub-dir files
$files = scanDir::scan($dirs, $file_ext, true);
?>
```
  

---

I needed to find a way to get the full path of all files in the directory and all subdirectories of a directory.<br>Here&apos;s my solution: Recursive functions!<br><br>

```
<?php
function find_all_files($dir)
{
    $root = scandir($dir);
    foreach($root as $value)
    {
        if($value === '.' || $value === '..') {continue;}
        if(is_file("$dir/$value")) {$result[]="$dir/$value";continue;}
        foreach(find_all_files("$dir/$value") as $value)
        {
            $result[]=$value;
        }
    }
    return $result;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.scandir.php)

**[To root](/README.md)**