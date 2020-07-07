# PUT method support





Hello PHP World After many Hours of worryness :=)



I have found the Solution for Resume or Pause Uploads

In this Code Snippet it is the Server Side not Client on any Desktop Programm you must use byte ranges to calculate the uploaded bytes and missing of total bytes.



Here the PHP Code





```
<?php

$CHUNK = 8192;



&#xA0; &#xA0; &#xA0; &#xA0; try {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!($putData = fopen(&quot;php://input&quot;, &quot;r&quot;)))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t get PUT data.&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // now the params can be used like any other variable

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // see below after input has finished



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tot_write = 0;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tmpFileName = &quot;/var/dev/tmp/PUT_FILE&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Create a temp file

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!is_file($tmpFileName)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose(fopen($tmpFileName, &quot;x&quot;)); //create the file and close it

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Open the file for writing

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!($fp = fopen($tmpFileName, &quot;w&quot;)))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t write to tmp file&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Read the data a chunk at a time and write to the file

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while ($data = fread($putData, $CHUNK)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $chunk_read = strlen($data);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (($block_write = fwrite($fp, $data)) != $chunk_read)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t write more to tmp file&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tot_write += $block_write;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!fclose($fp))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t close tmp file&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($putData);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Open the file for writing

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!($fp = fopen($tmpFileName, &quot;a&quot;)))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t write to tmp file&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Read the data a chunk at a time and write to the file

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while ($data = fread($putData, $CHUNK)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $chunk_read = strlen($data);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (($block_write = fwrite($fp, $data)) != $chunk_read)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t write more to tmp file&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tot_write += $block_write;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!fclose($fp))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Can&apos;t close tmp file&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($putData);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Check file length and MD5

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($tot_write != $file_size)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Wrong file size&quot;);



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $md5_arr = explode(&apos; &apos;, exec(&quot;md5sum $tmpFileName&quot;));

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $md5 = $md5sum_arr[0];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($md5 != $md5sum)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Wrong md5&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; } catch (Exception $e) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&apos;, $e-&gt;getMessage(), &quot;\n&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; }

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.put-method.php)

**[To root](/README.md)**