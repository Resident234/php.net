# ssh2_connect




<div class="phpcode"><span class="html">
Due to a lack of complete examples, here&apos;s a simple SSH2 class for connecting to a server, authenticating with public key authentication, verifying the server&apos;s fingerprint, issuing commands and reading their STDOUT and properly disconnecting.&#xA0; Note: You may need to make sure you commands produce output so the response can be pulled.&#xA0; Some people suggest that the command is not executed until you pull the response back.
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">NiceSSH </span><span class="keyword">{
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Host
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_host </span><span class="keyword">= </span><span class="string">&apos;myserver.example.com&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Port
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_port </span><span class="keyword">= </span><span class="default">22</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Server Fingerprint
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_server_fp </span><span class="keyword">= </span><span class="string">&apos;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Username
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_auth_user </span><span class="keyword">= </span><span class="string">&apos;username&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Public Key File
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_auth_pub </span><span class="keyword">= </span><span class="string">&apos;/home/username/.ssh/id_rsa.pub&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Private Key File
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_auth_priv </span><span class="keyword">= </span><span class="string">&apos;/home/username/.ssh/id_rsa&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Private Key Passphrase (null == no passphrase)
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$ssh_auth_pass</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// SSH Connection
<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$connection</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">connect</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection </span><span class="keyword">= </span><span class="default">ssh2_connect</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_host</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_port</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Cannot connect to server&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fingerprint </span><span class="keyword">= </span><span class="default">ssh2_fingerprint</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">, </span><span class="default">SSH2_FINGERPRINT_MD5 </span><span class="keyword">| </span><span class="default">SSH2_FINGERPRINT_HEX</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_server_fp</span><span class="keyword">, </span><span class="default">$fingerprint</span><span class="keyword">) !== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Unable to verify server identity!&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">ssh2_auth_pubkey_file</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_auth_user</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_auth_pub</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_auth_priv</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">ssh_auth_pass</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Autentication rejected by server&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; public function </span><span class="default">exec</span><span class="keyword">(</span><span class="default">$cmd</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!(</span><span class="default">$stream </span><span class="keyword">= </span><span class="default">ssh2_exec</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">, </span><span class="default">$cmd</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;SSH command failed&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">stream_set_blocking</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; while (</span><span class="default">$buf </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">, </span><span class="default">4096</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">.= </span><span class="default">$buf</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$data</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; public function </span><span class="default">disconnect</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">exec</span><span class="keyword">(</span><span class="string">&apos;echo &quot;EXITING&quot; &amp;&amp; exit;&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; public function </span><span class="default">__destruct</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">disconnect</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>[EDIT BY danbrown AT php DOT net: Contains two bugfixes suggested by &apos;AlainC&apos; in user note #109185 (removed) on 26-JUN-2012.]</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful when providing a specific hostkey order. <br><br><span class="default">&lt;?php<br>ssh2_connect</span><span class="keyword">(</span><span class="string">&apos;IP&apos;</span><span class="keyword">, </span><span class="string">&apos;port&apos;</span><span class="keyword">, array(</span><span class="string">&apos;hostkey&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;ssh-rsa, ssh-dss&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Will only work when the public key of the server is RSA, and not DSA also as expected. This is caused by the empty space before the &quot;ssh-dss&quot;. <br><br>So a similar code:<br><br><span class="default">&lt;?php<br>ssh2_connect</span><span class="keyword">(</span><span class="string">&apos;IP&apos;</span><span class="keyword">, </span><span class="string">&apos;port&apos;</span><span class="keyword">,&#xA0;&#xA0; array(</span><span class="string">&apos;hostkey&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;ssh-rsa,ssh-dss&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Will work. The HOSTKEY method is overriden using exactly what you write, so no empty spaces are allowed.<br><br>This took me some time that you could save ;)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ssh2-connect.php)

**[⬆ to root](/)**