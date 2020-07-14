# FTP context options



This is an example of how to allow fopen() to overwrite a file on an FTP site. If the stream context is not modified, an error will occur: "...failed to open stream: Remote file already exists and overwrite context option not specified...".<br><br>

```
<?php
// The path to the FTP file, including login arguments
$ftp_path = 'ftp://username:password@example.com/example.txt';

// Allows overwriting of existing files on the remote FTP server
$stream_options = array('ftp' => array('overwrite' => true));

// Creates a stream context resource with the defined options
$stream_context = stream_context_create($stream_options);

// Opens the file for writing and truncates it to zero length 
if ($fh = fopen($ftp_path, 'w', 0, $stream_context))
{
    // Writes contents to the file
    fputs($fh, 'example contents');
    
    // Closes the file handle
    fclose($fh);
}
else
{
    die('Could not open file.');
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/context.ftp.php)

**[To root](/README.md)**