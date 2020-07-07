# ssh2_sftp




<div class="phpcode"><span class="html">
Here is an example of how to send a file with SFTP:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">SFTPConnection<br></span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$connection</span><span class="keyword">;<br>&#xA0; &#xA0; private </span><span class="default">$sftp</span><span class="keyword">;<br><br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">=</span><span class="default">22</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection </span><span class="keyword">= @</span><span class="default">ssh2_connect</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (! </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Could not connect to </span><span class="default">$host</span><span class="string"> on port </span><span class="default">$port</span><span class="string">.&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">login</span><span class="keyword">(</span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (! @</span><span class="default">ssh2_auth_password</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">, </span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Could not authenticate with username </span><span class="default">$username</span><span class="string"> &quot; </span><span class="keyword">.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;and password </span><span class="default">$password</span><span class="string">.&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">sftp </span><span class="keyword">= @</span><span class="default">ssh2_sftp</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (! </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">sftp</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Could not initialize SFTP subsystem.&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">uploadFile</span><span class="keyword">(</span><span class="default">$local_file</span><span class="keyword">, </span><span class="default">$remote_file</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$sftp </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">sftp</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$stream </span><span class="keyword">= @</span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;ssh2.s<a href="ftp://" rel="nofollow" target="_blank">ftp://</a></span><span class="default">$sftp$remote_file</span><span class="string">&quot;</span><span class="keyword">, </span><span class="string">&apos;w&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (! </span><span class="default">$stream</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Could not open file: </span><span class="default">$remote_file</span><span class="string">&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data_to_send </span><span class="keyword">= @</span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$local_file</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$data_to_send </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Could not open local file: </span><span class="default">$local_file</span><span class="string">.&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (@</span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">, </span><span class="default">$data_to_send</span><span class="keyword">) === </span><span class="default">false</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Could not send data from file: </span><span class="default">$local_file</span><span class="string">.&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; @</span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br><br>try<br>{<br>&#xA0; &#xA0; </span><span class="default">$sftp </span><span class="keyword">= new </span><span class="default">SFTPConnection</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">, </span><span class="default">22</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$sftp</span><span class="keyword">-&gt;</span><span class="default">login</span><span class="keyword">(</span><span class="string">&quot;username&quot;</span><span class="keyword">, </span><span class="string">&quot;password&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$sftp</span><span class="keyword">-&gt;</span><span class="default">uploadFile</span><span class="keyword">(</span><span class="string">&quot;/tmp/to_be_sent&quot;</span><span class="keyword">, </span><span class="string">&quot;/tmp/to_be_received&quot;</span><span class="keyword">);<br>}<br>catch (</span><span class="default">Exception $e</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; echo </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">() . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ssh2-sftp.php)

**[To root](/README.md)**