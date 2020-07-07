# dir





Here my solution how to do effective recursiv directory listing.

Have fun.



```
<?php

/**
 * example of use: 
 */
$d = new RecDir(&quot;/etc/&quot;,false);
echo &quot;Path: &quot; . $d-&gt;getRootPath() . &quot;\n&quot;;
while (false !== ($entry = $d-&gt;read())) {
&#xA0;&#xA0; echo $entry.&quot;\n&quot;;
}
$d-&gt;close();

class RecDir
{
&#xA0;&#xA0; protected $currentPath;
&#xA0;&#xA0; protected $slash;
&#xA0;&#xA0; protected $rootPath;
&#xA0;&#xA0; protected $recursiveTree;&#xA0;&#xA0; 

&#xA0;&#xA0; function __construct($rootPath,$win=false)
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; switch($win)
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case true:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;slash = &apos;\\&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;slash = &apos;/&apos;;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; $this-&gt;rootPath = $rootPath;
&#xA0; &#xA0; &#xA0; $this-&gt;currentPath = $rootPath;
&#xA0; &#xA0; &#xA0; $this-&gt;recursiveTree = array(dir($this-&gt;rootPath));
&#xA0; &#xA0; &#xA0; $this-&gt;rewind();
&#xA0;&#xA0; }

&#xA0;&#xA0; function __destruct()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;close();
&#xA0;&#xA0; }

&#xA0;&#xA0; public function close()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; while(true === ($d = array_pop($this-&gt;recursiveTree)))
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $d-&gt;close();
&#xA0; &#xA0; &#xA0; }
&#xA0;&#xA0; }

&#xA0;&#xA0; public function closeChildren()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; while(count($this-&gt;recursiveTree)&gt;1 &amp;&amp; false !== ($d = array_pop($this-&gt;recursiveTree)))
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $d-&gt;close();
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return true;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; return false;
&#xA0;&#xA0; }

&#xA0;&#xA0; public function getRootPath()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; if(isset($this-&gt;rootPath))
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return $this-&gt;rootPath;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; return false;
&#xA0;&#xA0; }

&#xA0;&#xA0; public function getCurrentPath()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; if(isset($this-&gt;currentPath))
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return $this-&gt;currentPath;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; return false;
&#xA0;&#xA0; }
&#xA0;&#xA0; 
&#xA0;&#xA0; public function read()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; while(count($this-&gt;recursiveTree)&gt;0)
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $d = end($this-&gt;recursiveTree);
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if((false !== ($entry = $d-&gt;read())))
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($entry!=&apos;.&apos; &amp;&amp; $entry!=&apos;..&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $path = $d-&gt;path.$entry;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if(is_file($path))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $path;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; elseif(is_dir($path.$this-&gt;slash))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;currentPath = $path.$this-&gt;slash;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($child = @dir($path.$this-&gt;slash))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;recursiveTree[] = $child;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_pop($this-&gt;recursiveTree)-&gt;close();
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; return false;
&#xA0;&#xA0; }

&#xA0;&#xA0; public function rewind()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;closeChildren();
&#xA0; &#xA0; &#xA0; $this-&gt;rewindCurrent();
&#xA0;&#xA0; }

&#xA0;&#xA0; public function rewindCurrent()
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; return end($this-&gt;recursiveTree)-&gt;rewind();
&#xA0;&#xA0; }
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.dir.php)

**[To root](/README.md)**