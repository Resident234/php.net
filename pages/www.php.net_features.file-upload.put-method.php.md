# PUT method support



Hello PHP World After many Hours of worryness :=)<br><br>I have found the Solution for Resume or Pause Uploads<br>In this Code Snippet it is the Server Side not Client on any Desktop Programm you must use byte ranges to calculate the uploaded bytes and missing of total bytes.<br><br>Here the PHP Code<br><br>

```
<?php
$CHUNK = 8192;

        try {
            if (!($putData = fopen("php://input", "r")))
                throw new Exception("Can't get PUT data.");

            // now the params can be used like any other variable
            // see below after input has finished

            $tot_write = 0;
            $tmpFileName = "/var/dev/tmp/PUT_FILE";
            // Create a temp file
            if (!is_file($tmpFileName)) {
                fclose(fopen($tmpFileName, "x")); //create the file and close it
                // Open the file for writing
                if (!($fp = fopen($tmpFileName, "w")))
                    throw new Exception("Can't write to tmp file");

                // Read the data a chunk at a time and write to the file
                while ($data = fread($putData, $CHUNK)) {
                    $chunk_read = strlen($data);
                    if (($block_write = fwrite($fp, $data)) != $chunk_read)
                        throw new Exception("Can't write more to tmp file");

                    $tot_write += $block_write;
                }

                if (!fclose($fp))
                    throw new Exception("Can't close tmp file");

                unset($putData);
            } else {
                // Open the file for writing
                if (!($fp = fopen($tmpFileName, "a")))
                    throw new Exception("Can't write to tmp file");

                // Read the data a chunk at a time and write to the file
                while ($data = fread($putData, $CHUNK)) {
                    $chunk_read = strlen($data);
                    if (($block_write = fwrite($fp, $data)) != $chunk_read)
                        throw new Exception("Can't write more to tmp file");

                    $tot_write += $block_write;
                }

                if (!fclose($fp))
                    throw new Exception("Can't close tmp file");

                unset($putData);
            }

            // Check file length and MD5
            if ($tot_write != $file_size)
                throw new Exception("Wrong file size");

            $md5_arr = explode(' ', exec("md5sum $tmpFileName"));
            $md5 = $md5sum_arr[0];
            if ($md5 != $md5sum)
                throw new Exception("Wrong md5");
        } catch (Exception $e) {
            echo '', $e->getMessage(), "\n";
        }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/features.file-upload.put-method.php)

**[To root](/README.md)**