# exec




<div class="phpcode"><span class="html">
This will execute $cmd in the background (no cmd window) without PHP waiting for it to finish, on both Windows and Unix.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">execInBackground</span><span class="keyword">(</span><span class="default">$cmd</span><span class="keyword">) {
<br>&#xA0; &#xA0; if (</span><span class="default">substr</span><span class="keyword">(</span><span class="default">php_uname</span><span class="keyword">(), </span><span class="default">0</span><span class="keyword">, </span><span class="default">7</span><span class="keyword">) == </span><span class="string">&quot;Windows&quot;</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">pclose</span><span class="keyword">(</span><span class="default">popen</span><span class="keyword">(</span><span class="string">&quot;start /B &quot;</span><span class="keyword">. </span><span class="default">$cmd</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">));&#xA0; 
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="default">$cmd </span><span class="keyword">. </span><span class="string">&quot; &gt; /dev/null &amp;&quot;</span><span class="keyword">);&#xA0;&#xA0; 
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
(This is for linux users only).<br><br>We know now how we can fork a process in linux with the &amp; operator.<br>And by using command: nohup MY_COMMAND &gt; /dev/null 2&gt;&amp;1 &amp; echo $! we can return the pid of the process.<br><br>This small class is made so you can keep in track of your created processes ( meaning start/stop/status ).<br><br>You may use it to start a process or join an exisiting PID process.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">// You may use status(), start(), and stop(). notice that start() method gets called automatically one time.<br>&#xA0; &#xA0; </span><span class="default">$process </span><span class="keyword">= new </span><span class="default">Process</span><span class="keyword">(</span><span class="string">&apos;ls -al&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// or if you got the pid, however here only the status() metod will work.<br>&#xA0; &#xA0; </span><span class="default">$process </span><span class="keyword">= new </span><span class="default">Process</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$process</span><span class="keyword">.</span><span class="default">setPid</span><span class="keyword">(</span><span class="default">my_pid</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">// Then you can start/stop/ check status of the job.<br>&#xA0; &#xA0; </span><span class="default">$process</span><span class="keyword">.</span><span class="default">stop</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$process</span><span class="keyword">.</span><span class="default">start</span><span class="keyword">();<br>&#xA0; &#xA0; if (</span><span class="default">$process</span><span class="keyword">.</span><span class="default">status</span><span class="keyword">()){<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;The process is currently running&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;The process is not running.&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="comment">/* An easy way to keep in track of external processes.<br> * Ever wanted to execute a process in php, but you still wanted to have somewhat controll of the process ? Well.. This is a way of doing it.<br> * @compability: Linux only. (Windows does not work).<br> * @author: Peec<br> */<br></span><span class="keyword">class </span><span class="default">Process</span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$pid</span><span class="keyword">;<br>&#xA0; &#xA0; private </span><span class="default">$command</span><span class="keyword">;<br><br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$cl</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$cl </span><span class="keyword">!= </span><span class="default">false</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">command </span><span class="keyword">= </span><span class="default">$cl</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">runCom</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; private function </span><span class="default">runCom</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$command </span><span class="keyword">= </span><span class="string">&apos;nohup &apos;</span><span class="keyword">.</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">command</span><span class="keyword">.</span><span class="string">&apos; &gt; /dev/null 2&gt;&amp;1 &amp; echo $!&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="default">$command </span><span class="keyword">,</span><span class="default">$op</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pid </span><span class="keyword">= (int)</span><span class="default">$op</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">setPid</span><span class="keyword">(</span><span class="default">$pid</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pid </span><span class="keyword">= </span><span class="default">$pid</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">getPid</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pid</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">status</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$command </span><span class="keyword">= </span><span class="string">&apos;ps -p &apos;</span><span class="keyword">.</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pid</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="default">$command</span><span class="keyword">,</span><span class="default">$op</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!isset(</span><span class="default">$op</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]))return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">start</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">command </span><span class="keyword">!= </span><span class="string">&apos;&apos;</span><span class="keyword">)</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">runCom</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; else return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">stop</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$command </span><span class="keyword">= </span><span class="string">&apos;kill &apos;</span><span class="keyword">.</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pid</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="default">$command</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">status</span><span class="keyword">() == </span><span class="default">false</span><span class="keyword">)return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Can&#x2019;t get the output from your exec&#x2019;d command to appear in the $output array?<br>Is it echo&#x2019;ing all over your shell instead?<br><br>Append &quot;2&gt;&amp;1&quot; to the end of your command, for example:<br><br>exec(&quot;xmllint --noout ~/desktop/test.xml 2&gt;&amp;1&quot;, $retArr, $retVal);<br><br>Will fill the array $retArr with the expected output; one line per array key.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I too wrestled with getting a program to run in the background in Windows while the script continues to execute.&#xA0; This method unlike the other solutions allows you to start any program minimized, maximized, or with no window at all.&#xA0; llbra@phpbrasil&apos;s solution does work but it sometimes produces an unwanted window on the desktop when you really want the task to run hidden.
<br>
<br>start Notepad.exe minimized in the background:
<br>
<br><span class="default">&lt;?php
<br>$WshShell </span><span class="keyword">= new </span><span class="default">COM</span><span class="keyword">(</span><span class="string">&quot;WScript.Shell&quot;</span><span class="keyword">);
<br></span><span class="default">$oExec </span><span class="keyword">= </span><span class="default">$WshShell</span><span class="keyword">-&gt;</span><span class="default">Run</span><span class="keyword">(</span><span class="string">&quot;notepad.exe&quot;</span><span class="keyword">, </span><span class="default">7</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>start a shell command invisible in the background:
<br><span class="default">&lt;?php
<br>$WshShell </span><span class="keyword">= new </span><span class="default">COM</span><span class="keyword">(</span><span class="string">&quot;WScript.Shell&quot;</span><span class="keyword">);
<br></span><span class="default">$oExec </span><span class="keyword">= </span><span class="default">$WshShell</span><span class="keyword">-&gt;</span><span class="default">Run</span><span class="keyword">(</span><span class="string">&quot;cmd /C dir /S %windir%&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>start MSPaint maximized and wait for you to close it before continuing the script:
<br><span class="default">&lt;?php
<br>$WshShell </span><span class="keyword">= new </span><span class="default">COM</span><span class="keyword">(</span><span class="string">&quot;WScript.Shell&quot;</span><span class="keyword">);
<br></span><span class="default">$oExec </span><span class="keyword">= </span><span class="default">$WshShell</span><span class="keyword">-&gt;</span><span class="default">Run</span><span class="keyword">(</span><span class="string">&quot;mspaint.exe&quot;</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>For more info on the Run() method go to:
<br><a href="http://msdn.microsoft.com/library/en-us/script56/html/wsMthRun.asp" rel="nofollow" target="_blank">http://msdn.microsoft.com/library/en-us/script56/html/wsMthRun.asp</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.exec.php)

**[⬆ to root](/)**