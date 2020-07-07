# file_put_contents





File put contents fails if you try to put a file in a directory that doesn&apos;t exist. This creates the directory.



```
<?php
&#xA0; &#xA0; function file_force_contents($dir, $contents){
&#xA0; &#xA0; &#xA0; &#xA0; $parts = explode(&apos;/&apos;, $dir);
&#xA0; &#xA0; &#xA0; &#xA0; $file = array_pop($parts);
&#xA0; &#xA0; &#xA0; &#xA0; $dir = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; foreach($parts as $part)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!is_dir($dir .= &quot;/$part&quot;)) mkdir($dir);
&#xA0; &#xA0; &#xA0; &#xA0; file_put_contents(&quot;$dir/$file&quot;, $contents);
&#xA0; &#xA0; }
?>
```



  

#



It should be obvious that this should only be used if you&apos;re making one write, if you are writing multiple times to the same file you should handle it yourself with fopen and fwrite, the fclose when you are done writing.

Benchmark below:

file_put_contents() for 1,000,000 writes - average of 3 benchmarks:

 real 0m3.932s
 user 0m2.487s
 sys 0m1.437s

fopen() fwrite() for 1,000,000 writes, fclose() -&#xA0; average of 3 benchmarks:

 real 0m2.265s
 user 0m1.819s
 sys 0m0.445s

  

#



A slightly simplified version of the method: http://php.net/manual/ru/function.file-put-contents.php#84180



```
<?php 
function file_force_contents( $fullPath, $contents, $flags = 0 ){
&#xA0; &#xA0; $parts = explode( &apos;/&apos;, $fullPath );
&#xA0; &#xA0; array_pop( $parts );
&#xA0; &#xA0; $dir = implode( &apos;/&apos;, $parts );
&#xA0; &#xA0; 
&#xA0; &#xA0; if( !is_dir( $dir ) )
&#xA0; &#xA0; &#xA0; &#xA0; mkdir( $dir, 0777, true );
&#xA0; &#xA0; 
&#xA0; &#xA0; file_put_contents( $fullPath, $contents, $flags );
}

file_force_contents( ROOT.&apos;/newpath/file.txt&apos;, &apos;message&apos;, LOCK_EX );
?>
```



  

#



Please note that when saving using an FTP host, an additional stream context must be passed through telling PHP to overwrite the file.





```
<?php

 /* set the FTP hostname */

 $user = &quot;test&quot;;

 $pass = &quot;myFTP&quot;;

 $host = &quot;example.com&quot;;

 $file = &quot;test.txt&quot;;

 $hostname = $user . &quot;:&quot; . $pass . &quot;@&quot; . $host . &quot;/&quot; . $file;



 /* the file content */

 $content = &quot;this is just a test.&quot;;

 

 /* create a stream context telling PHP to overwrite the file */

 $options = array(&apos;ftp&apos; =&gt; array(&apos;overwrite&apos; =&gt; true));

 $stream = stream_context_create($options);

 

 /* and finally, put the contents */

 file_put_contents($hostname, $content, 0, $stream);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.file-put-contents.php)

**[To root](/README.md)**