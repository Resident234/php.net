# file_put_contents



File put contents fails if you try to put a file in a directory that doesn&apos;t exist. This creates the directory.<br><br>

```
<?php
    function file_force_contents($dir, $contents){
        $parts = explode(&apos;/&apos;, $dir);
        $file = array_pop($parts);
        $dir = &apos;&apos;;
        foreach($parts as $part)
            if(!is_dir($dir .= "/$part")) mkdir($dir);
        file_put_contents("$dir/$file", $contents);
    }
?>
```
  

#

It should be obvious that this should only be used if you&apos;re making one write, if you are writing multiple times to the same file you should handle it yourself with fopen and fwrite, the fclose when you are done writing.<br><br>Benchmark below:<br><br>file_put_contents() for 1,000,000 writes - average of 3 benchmarks:<br><br> real 0m3.932s<br> user 0m2.487s<br> sys 0m1.437s<br><br>fopen() fwrite() for 1,000,000 writes, fclose() -  average of 3 benchmarks:<br><br> real 0m2.265s<br> user 0m1.819s<br> sys 0m0.445s  

#

A slightly simplified version of the method: http://php.net/manual/ru/function.file-put-contents.php#84180<br><br>

```
<?php 
function file_force_contents( $fullPath, $contents, $flags = 0 ){
    $parts = explode( &apos;/&apos;, $fullPath );
    array_pop( $parts );
    $dir = implode( &apos;/&apos;, $parts );
    
    if( !is_dir( $dir ) )
        mkdir( $dir, 0777, true );
    
    file_put_contents( $fullPath, $contents, $flags );
}

file_force_contents( ROOT.&apos;/newpath/file.txt&apos;, &apos;message&apos;, LOCK_EX );
?>
```
  

#

Please note that when saving using an FTP host, an additional stream context must be passed through telling PHP to overwrite the file.<br><br>

```
<?php
 /* set the FTP hostname */
 $user = "test";
 $pass = "myFTP";
 $host = "example.com";
 $file = "test.txt";
 $hostname = $user . ":" . $pass . "@" . $host . "/" . $file;

 /* the file content */
 $content = "this is just a test.";
 
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