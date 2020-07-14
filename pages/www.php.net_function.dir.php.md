# dir



Here my solution how to do effective recursiv directory listing.<br><br>Have fun.<br><br>

```
<?php

/**
 * example of use: 
 */
$d = new RecDir("/etc/",false);
echo "Path: " . $d-&gt;getRootPath() . "\n";
while (false !== ($entry = $d-&gt;read())) {
   echo $entry."\n";
}
$d-&gt;close();

class RecDir
{
   protected $currentPath;
   protected $slash;
   protected $rootPath;
   protected $recursiveTree;   

   function __construct($rootPath,$win=false)
   {
      switch($win)
      {
         case true:
            $this-&gt;slash = &apos;\\&apos;;
            break;
         default:
            $this-&gt;slash = &apos;/&apos;;
      }
      $this-&gt;rootPath = $rootPath;
      $this-&gt;currentPath = $rootPath;
      $this-&gt;recursiveTree = array(dir($this-&gt;rootPath));
      $this-&gt;rewind();
   }

   function __destruct()
   {
      $this-&gt;close();
   }

   public function close()
   {
      while(true === ($d = array_pop($this-&gt;recursiveTree)))
      {
         $d-&gt;close();
      }
   }

   public function closeChildren()
   {
      while(count($this-&gt;recursiveTree)&gt;1 &amp;&amp; false !== ($d = array_pop($this-&gt;recursiveTree)))
      {
         $d-&gt;close();
         return true;
      }
      return false;
   }

   public function getRootPath()
   {
      if(isset($this-&gt;rootPath))
      {
         return $this-&gt;rootPath;
      }
      return false;
   }

   public function getCurrentPath()
   {
      if(isset($this-&gt;currentPath))
      {
         return $this-&gt;currentPath;
      }
      return false;
   }
   
   public function read()
   {
      while(count($this-&gt;recursiveTree)&gt;0)
      {
         $d = end($this-&gt;recursiveTree);
         if((false !== ($entry = $d-&gt;read())))
         {
            if($entry!=&apos;.&apos; &amp;&amp; $entry!=&apos;..&apos;)
            {
               $path = $d-&gt;path.$entry;
               
               if(is_file($path))
               {
                  return $path;
               }
               elseif(is_dir($path.$this-&gt;slash))
               {
                  $this-&gt;currentPath = $path.$this-&gt;slash;
                  if($child = @dir($path.$this-&gt;slash))
                  {
                     $this-&gt;recursiveTree[] = $child;
                  }
               }
            }
         }
         else
         {
            array_pop($this-&gt;recursiveTree)-&gt;close();
         }
      }
      return false;
   }

   public function rewind()
   {
      $this-&gt;closeChildren();
      $this-&gt;rewindCurrent();
   }

   public function rewindCurrent()
   {
      return end($this-&gt;recursiveTree)-&gt;rewind();
   }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.dir.php)

**[To root](/README.md)**