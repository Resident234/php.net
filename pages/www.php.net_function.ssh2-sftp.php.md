# ssh2_sftp



Here is an example of how to send a file with SFTP:<br><br>

```
<?php

class SFTPConnection
{
    private $connection;
    private $sftp;

    public function __construct($host, $port=22)
    {
        $this-&gt;connection = @ssh2_connect($host, $port);
        if (! $this-&gt;connection)
            throw new Exception("Could not connect to $host on port $port.");
    }

    public function login($username, $password)
    {
        if (! @ssh2_auth_password($this-&gt;connection, $username, $password))
            throw new Exception("Could not authenticate with username $username " .
                                "and password $password.");

        $this-&gt;sftp = @ssh2_sftp($this-&gt;connection);
        if (! $this-&gt;sftp)
            throw new Exception("Could not initialize SFTP subsystem.");
    }

    public function uploadFile($local_file, $remote_file)
    {
        $sftp = $this-&gt;sftp;
        $stream = @fopen("ssh2.sftp://$sftp$remote_file", &apos;w&apos;);

        if (! $stream)
            throw new Exception("Could not open file: $remote_file");

        $data_to_send = @file_get_contents($local_file);
        if ($data_to_send === false)
            throw new Exception("Could not open local file: $local_file.");

        if (@fwrite($stream, $data_to_send) === false)
            throw new Exception("Could not send data from file: $local_file.");

        @fclose($stream);
    }
}

try
{
    $sftp = new SFTPConnection("localhost", 22);
    $sftp-&gt;login("username", "password");
    $sftp-&gt;uploadFile("/tmp/to_be_sent", "/tmp/to_be_received");
}
catch (Exception $e)
{
    echo $e-&gt;getMessage() . "\n";
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.ssh2-sftp.php)

**[To root](/README.md)**