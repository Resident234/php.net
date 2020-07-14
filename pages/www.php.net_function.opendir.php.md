# opendir



Sometimes the programmer needs to access folder content which has arabic name but the opendir function will return null resources id<br><br>for that we must convert the dirname charset from utf-8 to windows-1256 by the iconv function just if the preg_match function detect arabic characters and use " U " additionality to enable multibyte matching<br><br>

```
<?php

$dir = ("./"); // on this file dir
      
// detect if the path has arabic characters and use " u "  optional to enable function to match multibyte characters

if (preg_match(&apos;#[\x{0600}-\x{06FF}]#iu&apos;, $dir) )  
{

    // convert input ( utf-8 ) to output ( windows-1256 ) 
    
    $dir = iconv("utf-8","windows-1256",$dir);
    
}

 if( is_dir($dir) ) 
 {
     
     
     if(  ( $dh = opendir($dir) ) !== null  ) 
     {
    
         
         while ( ( $file = readdir($dh) ) !== false  ) 
         {
             
             
             echo "filename: ".$file ." filetype : ".filetype($dir.$file)."&lt;br/&gt;";
             
             
         }
        
     }
     
     
 }

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.opendir.php)

**[To root](/README.md)**