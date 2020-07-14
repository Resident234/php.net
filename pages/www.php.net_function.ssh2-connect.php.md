# ssh2_connect



Due to a lack of complete examples, here&apos;s a simple SSH2 class for connecting to a server, authenticating with public key authentication, verifying the server&apos;s fingerprint, issuing commands and reading their STDOUT and properly disconnecting.  Note: You may need to make sure you commands produce output so the response can be pulled.  Some people suggest that the command is not executed until you pull the response back.<br>

```
<?php
class NiceSSH {
    // SSH Host
    private $ssh_host = &apos;myserver.example.com&apos;;
    // SSH Port
    private $ssh_port = 22;
    // SSH Server Fingerprint
    private $ssh_server_fp = &apos;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&apos;;
    // SSH Username
    private $ssh_auth_user = &apos;username&apos;;
    // SSH Public Key File
    private $ssh_auth_pub = &apos;/home/username/.ssh/id_rsa.pub&apos;;
    // SSH Private Key File
    private $ssh_auth_priv = &apos;/home/username/.ssh/id_rsa&apos;;
    // SSH Private Key Passphrase (null == no passphrase)
    private $ssh_auth_pass;
    // SSH Connection
    private $connection;
    
    public function connect() {
        if (!($this-&gt;connection = ssh2_connect($this-&gt;ssh_host, $this-&gt;ssh_port))) {
            throw new Exception(&apos;Cannot connect to server&apos;);
        }
        $fingerprint = ssh2_fingerprint($this-&gt;connection, SSH2_FINGERPRINT_MD5 | SSH2_FINGERPRINT_HEX);
        if (strcmp($this-&gt;ssh_server_fp, $fingerprint) !== 0) {
            throw new Exception(&apos;Unable to verify server identity!&apos;);
        }
        if (!ssh2_auth_pubkey_file($this-&gt;connection, $this-&gt;ssh_auth_user, $this-&gt;ssh_auth_pub, $this-&gt;ssh_auth_priv, $this-&gt;ssh_auth_pass)) {
            throw new Exception(&apos;Autentication rejected by server&apos;);
        }
    }
    public function exec($cmd) {
        if (!($stream = ssh2_exec($this-&gt;connection, $cmd))) {
            throw new Exception(&apos;SSH command failed&apos;);
        }
        stream_set_blocking($stream, true);
        $data = "";
        while ($buf = fread($stream, 4096)) {
            $data .= $buf;
        }
        fclose($stream);
        return $data;
    }
    public function disconnect() {
        $this-&gt;exec(&apos;echo "EXITING" &amp;&amp; exit;&apos;);
        $this-&gt;connection = null;
    }
    public function __destruct() {
        $this-&gt;disconnect();
    }
}
?>
```
<br><br>[EDIT BY danbrown AT php DOT net: Contains two bugfixes suggested by &apos;AlainC&apos; in user note #109185 (removed) on 26-JUN-2012.]  

#

Be careful when providing a specific hostkey order. <br><br>

```
<?php
ssh2_connect(&apos;IP&apos;, &apos;port&apos;, array(&apos;hostkey&apos;=&gt;&apos;ssh-rsa, ssh-dss&apos;));
?>
```


Will only work when the public key of the server is RSA, and not DSA also as expected. This is caused by the empty space before the "ssh-dss". 

So a similar code:



```
<?php
ssh2_connect(&apos;IP&apos;, &apos;port&apos;,   array(&apos;hostkey&apos;=&gt;&apos;ssh-rsa,ssh-dss&apos;));
?>
```
<br><br>Will work. The HOSTKEY method is overriden using exactly what you write, so no empty spaces are allowed.<br><br>This took me some time that you could save ;)  

#

[Official documentation page](https://www.php.net/manual/en/function.ssh2-connect.php)

**[To root](/README.md)**