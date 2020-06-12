# Built-in web server




<div class="phpcode"><span class="html">
In order to set project specific configuration options, simply add a php.ini file to your project, and then run the built-in server with this flag:<br><br>php -S localhost:8000 -c php.ini<br><br>This is especially helpful for settings that cannot be set at runtime (ini_set()).</span>
</div>
  

#


<div class="phpcode"><span class="html">
I painfully experienced behaviour that I can&apos;t seem to find documented here so I wanted to save everyone from repeating my mistake by giving the following heads up:<br><br>When starting php -S on a mac (in my case macOS Sierra) to host a local server, I had trouble with connecting from legacy Java. <br><br>As it turned out, if you started the php server with <br>&quot;php -S localhost:80&quot; <br>the server will be started with ipv6 support only!<br><br>To access it via ipv4, you need to change the start up command like so:<br> &quot;php -S 127.0.0.1:80&quot;<br>which starts server in ipv4 mode only.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&#x2019;s not mentioned directly, and may not be obvious, but you can also use this to create a virtual host. This, of course, requires the help of your hosts file.<br><br>Here are the steps:<br><br>1&#xA0; &#xA0; /etc/hosts<br>&#xA0; &#xA0; 127.0.0.1&#xA0; &#xA0; www.example.com<br><br>2&#xA0; &#xA0; cd [root folder]<br>&#xA0; &#xA0; php -S www.example.com:8000<br><br>3&#xA0; &#xA0; Browser:<br>&#xA0; &#xA0; <a href="http://www.example.com:8000/index.php" rel="nofollow" target="_blank">http://www.example.com:8000/index.php</a><br><br>Combined with a simple SQLite database, you have a very handy testing environment.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If your URI contains a dot, you&apos;ll lose the $_SERVER[&apos;PATH_INFO&apos;] variable, when using the built-in webserver.<br>I wanted to write an API, and use .json ending in the URI-s, but then the framework&apos;s routing mechanism broke, and it took a lot of time to discover that the reason behind it was its router relying on $_SERVER[&apos;PATH_INFO&apos;].<br><br>References:<br><a href="https://bugs.php.net/bug.php?id=61286" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=61286</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
On Windows you may find useful to have a phpserver.bat file in shell:sendto with the folowing:<br>explorer <a href="http://localhost:8888" rel="nofollow" target="_blank">http://localhost:8888</a><br>rem check if arg is file or dir<br>if exist &quot;%~1\&quot; (<br>&#xA0; php -S localhost:8888 -t &quot;%~1&quot;<br>) else (<br>&#xA0; php -S localhost:8888 -t &quot;%~dp1&quot;<br>)<br><br>then for fast web testing you only have to SendTo a file or folder to this bat and it will open your explorer and run the server.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To output debugging information on the command line you can write output to php://stdout:<br><br><span class="default">&lt;?php<br>$path </span><span class="keyword">= </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&quot;SCRIPT_FILENAME&quot;</span><span class="keyword">];<br><br></span><span class="default">file_put_contents</span><span class="keyword">(</span><span class="string">&quot;php://stdout&quot;</span><span class="keyword">, </span><span class="string">&quot;\nRequested: </span><span class="default">$path</span><span class="string">&quot;</span><span class="keyword">);<br>echo </span><span class="string">&quot;&lt;p&gt;Hello World&lt;/p&gt;&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.webserver.php)

**[To root](/)**