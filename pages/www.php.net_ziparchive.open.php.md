# ZipArchive::open



With php 5.2.6, the following code created a new zip or replaced a existing zip.<br>Note that I am only using the ZIPARCHIVE::OVERWRITE flag.<br><br>

```
<?php
    $zip = new ZipArchive();
    $opened = $zip-&gt;open( $zipFileName, ZIPARCHIVE::OVERWRITE );
    if( $opened !== true ){
        die("cannot open {$zipFileName} for writing.");
    }
    $zip-&gt;addFromString( $name, $contents );
    $zip-&gt;close();
?>
```


Now, with php 5.2.8, it does not work and gives this warning:

Warning: ZipArchive::addFromString() [ziparchive.addfromstring]: Invalid or unitialized Zip object in [myfile] on line [myline]

To fix this, you must specify the flags as create or overwrite.



```
<?php
    $zip = new ZipArchive();
    $opened = $zip-&gt;open( $zipFileName, ZIPARCHIVE::CREATE | ZIPARCHIVE::OVERWRITE );
    if( $opened !== true ){
        die("cannot open {$zipFileName} for writing.");
    }
    $zip-&gt;addFromString( $name, $contents );
    $zip-&gt;close();
?>
```
<br><br>When googling for the error message I found a lot of people that had it but couldn&apos;t figure out why they were getting it.<br>I hope this helps someone.  

#



```
<?php
#made by abolfazl ziaratban (c)
#license GPL

class zip extends ZipArchive
    {
        public function message($code)
            {
                switch ($code)
                    {
                        case 0:
                        return &apos;No error&apos;;
                        
                        case 1:
                        return &apos;Multi-disk zip archives not supported&apos;;
                        
                        case 2:
                        return &apos;Renaming temporary file failed&apos;;
                        
                        case 3:
                        return &apos;Closing zip archive failed&apos;;
                        
                        case 4:
                        return &apos;Seek error&apos;;
                        
                        case 5:
                        return &apos;Read error&apos;;
                        
                        case 6:
                        return &apos;Write error&apos;;
                        
                        case 7:
                        return &apos;CRC error&apos;;
                        
                        case 8:
                        return &apos;Containing zip archive was closed&apos;;
                        
                        case 9:
                        return &apos;No such file&apos;;
                        
                        case 10:
                        return &apos;File already exists&apos;;
                        
                        case 11:
                        return &apos;Can\&apos;t open file&apos;;
                        
                        case 12:
                        return &apos;Failure to create temporary file&apos;;
                        
                        case 13:
                        return &apos;Zlib error&apos;;
                        
                        case 14:
                        return &apos;Malloc failure&apos;;
                        
                        case 15:
                        return &apos;Entry has been changed&apos;;
                        
                        case 16:
                        return &apos;Compression method not supported&apos;;
                        
                        case 17:
                        return &apos;Premature EOF&apos;;
                        
                        case 18:
                        return &apos;Invalid argument&apos;;
                        
                        case 19:
                        return &apos;Not a zip archive&apos;;
                        
                        case 20:
                        return &apos;Internal error&apos;;
                        
                        case 21:
                        return &apos;Zip archive inconsistent&apos;;
                        
                        case 22:
                        return &apos;Can\&apos;t remove file&apos;;
                        
                        case 23:
                        return &apos;Entry has been deleted&apos;;
                        
                        default:
                        return &apos;An unknown error has occurred(&apos;.intval($code).&apos;)&apos;;
                    }                
            }

        public function isDir($path)
            {
                return substr($path,-1) == &apos;/&apos;;
            }

        public function getTree()
            {
                $Tree = array();
                $pathArray = array();
                for($i=0; $i&lt;$this-&gt;numFiles; $i++)
                    {
                        $path = $this-&gt;getNameIndex($i);
                        $pathBySlash = array_values(explode(&apos;/&apos;,$path));
                        $c = count($pathBySlash);
                        $temp = &amp;$Tree;
                        for($j=0; $j&lt;$c-1; $j++)
                            if(isset($temp[$pathBySlash[$j]]))
                                $temp = &amp;$temp[$pathBySlash[$j]];
                            else
                                {
                                    $temp[$pathBySlash[$j]] = array();
                                    $temp = &amp;$temp[$pathBySlash[$j]];
                                }
                        if($this-&gt;isDir($path))
                            $temp[$pathBySlash[$c-1]] = array();
                        else
                            $temp[] = $pathBySlash[$c-1];
                    }
                return $Tree;
            }
    }
?>
```
  

#



```
<?php
    // Use ZipArchive::OVERWRITE when the targetd file does not exist may lead you to an error like this
    // Warning: ZipArchive::addFile(): Invalid or uninitialized Zip object 
    // try ZipArchive::OVERWRITE|ZipArchive::CREATE when you want to replace a zip archive that may not exist
    $zip = new ZipArchive;
    $rt=$zip-&gt;open(&apos;i.zip&apos;,ZipArchive::OVERWRITE);
    echo $rt;
    // when i.zip does not exist, $rt is 9, ZipArchive::ER_NOENT, or "No such file."
    $zip-&gt;addFile(&apos;wuxiancheng.cn.sql&apos;,&apos;db.sql&apos;);
    // triggers an error with the message "Warning: ZipArchive::addFile(): Invalid or uninitialized Zip object ..."
    
    
    // Use ZipArchive::OVERWRITE|ZipArchive::CREATE
    $zip = new ZipArchive;
    $zip-&gt;open(&apos;i.zip&apos;,ZipArchive::OVERWRITE|ZipArchive::CREATE);    
    $zip-&gt;addFile(&apos;wuxiancheng.cn.sql&apos;,&apos;db.sql&apos;);    
?>
```
  

#

if you are echoing out the output and confused about the number...maybe this will help.  i&apos;m not totally sure it is accurate though.<br><br>ZIPARCHIVE::ER_EXISTS - 10<br>ZIPARCHIVE::ER_INCONS - 21<br>ZIPARCHIVE::ER_INVAL - 18<br>ZIPARCHIVE::ER_MEMORY - 14<br>ZIPARCHIVE::ER_NOENT - 9<br>ZIPARCHIVE::ER_NOZIP - 19<br>ZIPARCHIVE::ER_OPEN - 11<br>ZIPARCHIVE::ER_READ - 5<br>ZIPARCHIVE::ER_SEEK - 4  

#

If you have archives that you want to overwrite just use:<br><br>ZIPARCHIVE::CREATE<br><br>It will overwrite existing archives and at the same time create new ones if they don&apos;t already exist.<br><br>ZIPARCHIVE::OVERWRITE won&apos;t work for both of these scenarios.<br><br>(PHP version 5.4.4)  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.open.php)

**[To root](/README.md)**