# ssh2_scp_send



In addition to my previous post, I figured out that sftp-&gt;fopen-&gt;file_get_contents-&gt;fwrite has much better performance than ssh2_scp_send.<br><br>I&apos;ve used the following code to test:<br><br>

```
<?php
$srcFile = '/var/tmp/dir1/file_to_send.txt';
$dstFile = '/var/tmp/dir2/file_received.txt';

// Create connection the the remote host
$conn = ssh2_connect('my.server.com', 22);

// Create SFTP session
$sftp = ssh2_sftp($conn);

$sftpStream = @fopen('ssh2.sftp://'.$sftp.$dstFile, 'w');

try {

    if (!$sftpStream) {
        throw new Exception("Could not open remote file: $dstFile");
    }
    
    $data_to_send = @file_get_contents($srcFile);
    
    if ($data_to_send === false) {
        throw new Exception("Could not open local file: $srcFile.");
    }
    
    if (@fwrite($sftpStream, $data_to_send) === false) {
        throw new Exception("Could not send data from file: $srcFile.");
    }
    
    fclose($sftpStream);
                    
} catch (Exception $e) {
    error_log('Exception: ' . $e->getMessage());
    fclose($sftpStream);
}
?>
```
<br><br>For the test I&apos;ve sent three files with total size of 6kB, and the times to send including connect to the server were:<br><br>SFTP -&gt; 15 sec.<br>ssh2_scp_send -&gt; 22 sec.<br><br>Cheers,<br><br>Pimmy  

---

[Official documentation page](https://www.php.net/manual/en/function.ssh2-scp-send.php)

**[To root](/README.md)**