# shell_exec




<div class="phpcode"><span class="html">
If you&apos;re trying to run a command such as &quot;gunzip -t&quot; in shell_exec and getting an empty result, you might need to add 2&gt;&amp;1 to the end of the command, eg:<br><br>Won&apos;t always work:<br>echo shell_exec(&quot;gunzip -c -t $path_to_backup_file&quot;);<br><br>Should work:<br>echo shell_exec(&quot;gunzip -c -t $path_to_backup_file 2&gt;&amp;1&quot;);<br><br>In the above example, a line break at the beginning of the gunzip output seemed to prevent shell_exec printing anything else. Hope this saves someone else an hour or two.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To run a command in background, the output must be redirected to /dev/null. This is written in exec() manual page. There are cases where you need the output to be logged somewhere else though. Redirecting the output to a file like this didn&apos;t work for me:<br><br><span class="default">&lt;?php<br></span><span class="comment"># this doesn&apos;t work!<br></span><span class="default">shell_exec</span><span class="keyword">(</span><span class="string">&quot;my_script.sh 2&gt;&amp;1 &gt;&gt; /tmp/mylog &amp;&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Using the above command still hangs web browser request.<br><br>Seems like you have to add exactly &quot;/dev/null&quot; to the command line. For instance, this worked:<br><br><span class="default">&lt;?php<br></span><span class="comment"># works, but output is lost<br></span><span class="default">shell_exec</span><span class="keyword">(</span><span class="string">&quot;my_script.sh 2&gt;/dev/null &gt;/dev/null &amp;&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>But I wanted the output, so I used this:<br><br><span class="default">&lt;?php<br>shell_exec</span><span class="keyword">(</span><span class="string">&quot;my_script.sh 2&gt;&amp;1 | tee -a /tmp/mylog 2&gt;/dev/null &gt;/dev/null &amp;&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Hope this helps someone.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.shell-exec.php)

**[⬆ to root](/)**