# ZipArchive::open



With php 5.2.6, the following code created a new zip or replaced a existing zip.<br>Note that I am only using the ZIPARCHIVE::OVERWRITE flag.<br><br>

```
<?php
    $zip = new ZipArchive();
    $opened = $zip->open( $zipFileName, ZIPARCHIVE::OVERWRITE );
    if( $opened !== true ){
        die("cannot open {$zipFileName} for writing.");
    }
    $zip->addFromString( $name, $contents );
    $zip->close();
?>
```


Now, with php 5.2.8, it does not work and gives this warning:

Warning: ZipArchive::addFromString() [ziparchive.addfromstring]: Invalid or unitialized Zip object in [myfile] on line [myline]

To fix this, you must specify the flags as create or overwrite.



```
<?php
    $zip = new ZipArchive();
    $opened = $zip->open( $zipFileName, ZIPARCHIVE::CREATE | ZIPARCHIVE::OVERWRITE );
    if( $opened !== true ){
        die("cannot open {$zipFileName} for writing.");
    }
    $zip->addFromString( $name, $contents );
    $zip->close();
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
                        return 'No error';
                        
                        case 1:
                        return 'Multi-disk zip archives not supported';
                        
                        case 2:
                        return 'Renaming temporary file failed';
                        
                        case 3:
                        return 'Closing zip archive failed';
                        
                        case 4:
                        return 'Seek error';
                        
                        case 5:
                        return 'Read error';
                        
                        case 6:
                        return 'Write error';
                        
                        case 7:
                        return 'CRC error';
                        
                        case 8:
                        return 'Containing zip archive was closed';
                        
                        case 9:
                        return 'No such file';
                        
                        case 10:
                        return 'File already exists';
                        
                        case 11:
                        return 'Can\'t open file';
                        
                        case 12:
                        return 'Failure to create temporary file';
                        
                        case 13:
                        return 'Zlib error';
                        
                        case 14:
                        return 'Malloc failure';
                        
                        case 15:
                        return 'Entry has been changed';
                        
                        case 16:
                        return 'Compression method not supported';
                        
                        case 17:
                        return 'Premature EOF';
                        
                        case 18:
                        return 'Invalid argument';
                        
                        case 19:
                        return 'Not a zip archive';
                        
                        case 20:
                        return 'Internal error';
                        
                        case 21:
                        return 'Zip archive inconsistent';
                        
                        case 22:
                        return 'Can\'t remove file';
                        
                        case 23:
                        return 'Entry has been deleted';
                        
                        default:
                        return 'An unknown error has occurred('.intval($code).')';
                    }                
            }

        public function isDir($path)
            {
                return substr($path,-1) == '/';
            }

        public function getTree()
            {
                $Tree = array();
                $pathArray = array();
                for($i=0; $i&lt;$this->numFiles; $i++)
                    {
                        $path = $this->getNameIndex($i);
                        $pathBySlash = array_values(explode('/',$path));
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
                        if($this->isDir($path))
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
    $rt=$zip->open('i.zip',ZipArchive::OVERWRITE);
    echo $rt;
    // when i.zip does not exist, $rt is 9, ZipArchive::ER_NOENT, or "No such file."
    $zip->addFile('wuxiancheng.cn.sql','db.sql');
    // triggers an error with the message "Warning: ZipArchive::addFile(): Invalid or uninitialized Zip object ..."
    
    
    // Use ZipArchive::OVERWRITE|ZipArchive::CREATE
    $zip = new ZipArchive;
    $zip->open('i.zip',ZipArchive::OVERWRITE|ZipArchive::CREATE);    
    $zip->addFile('wuxiancheng.cn.sql','db.sql');    
?>
```
  

#

if you are echoing out the output and confused about the number...maybe this will help.  i&apos;m not totally sure it is accurate though.<br><br>ZIPARCHIVE::ER_EXISTS - 10<br>ZIPARCHIVE::ER_INCONS - 21<br>ZIPARCHIVE::ER_INVAL - 18<br>ZIPARCHIVE::ER_MEMORY - 14<br>ZIPARCHIVE::ER_NOENT - 9<br>ZIPARCHIVE::ER_NOZIP - 19<br>ZIPARCHIVE::ER_OPEN - 11<br>ZIPARCHIVE::ER_READ - 5<br>ZIPARCHIVE::ER_SEEK - 4  

#

If you have archives that you want to overwrite just use:<br><br>ZIPARCHIVE::CREATE<br><br>It will overwrite existing archives and at the same time create new ones if they don&apos;t already exist.<br><br>ZIPARCHIVE::OVERWRITE won&apos;t work for both of these scenarios.<br><br>(PHP version 5.4.4)  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.open.php)

**[To root](/README.md)**