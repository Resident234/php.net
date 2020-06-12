# pcntl_fork




<div class="phpcode"><span class="html">
&quot;Fatal Error&quot; has always been the bane of my world because there is no way to capture and handle the condition in PHP. My team builds almost everything in PHP in order to leverage our core library of code, so it was of the essence to find a solution for this problem of scripts bombing unrecoverably and us never knowing about it.<br><br>One of our background automation systems creates a &quot;task queue&quot; of sorts and for each task in the queue, a PHP module is include()ed to handle the task. Sometimes however a poorly behaved module will nuke with a Fatal Error and take out the parent script with it.<br><br>I decided to try to use pcntl_fork() to isolate the task module from the parent code, and it seems to work: a Fatal Error generated within the module makes the child task bomb, and the waiting parent can simply catch the return code from the child and track/alert us to the problem as needed. <br><br>Naturally something similar could be done if I wanted to simply exec() the module and check the output, but then I would not have the benefit of the stateful environment that the parent script has so carefully prepared. This allows me to keep the child process within the context of the parent&apos;s running environment and not suffer the consequences of Fatal Errors stopping the task queue from continuing to process.<br><br>Here is fork_n_wait.php for your amusement:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">if (! </span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;pcntl_fork&apos;</span><span class="keyword">)) die(</span><span class="string">&apos;PCNTL functions not available on this PHP installation&apos;</span><span class="keyword">);<br><br>for (</span><span class="default">$x </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$x </span><span class="keyword">&lt; </span><span class="default">5</span><span class="keyword">; </span><span class="default">$x</span><span class="keyword">++) {<br>&#xA0;&#xA0; switch (</span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; case -</span><span class="default">1</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// @fail<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">die(</span><span class="string">&apos;Fork failed&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br><br>&#xA0; &#xA0; &#xA0; case </span><span class="default">0</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// @child: Include() misbehaving code here<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">print </span><span class="string">&quot;FORK: Child #</span><span class="keyword">{</span><span class="default">$x</span><span class="keyword">}</span><span class="string"> preparing to nuke...\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">generate_fatal_error</span><span class="keyword">(); </span><span class="comment">// Undefined function<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">break;<br><br>&#xA0; &#xA0; &#xA0; default:<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// @parent<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">print </span><span class="string">&quot;FORK: Parent, letting the child run amok...\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">pcntl_waitpid</span><span class="keyword">(</span><span class="default">$pid</span><span class="keyword">, </span><span class="default">$status</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;<br>&#xA0;&#xA0; }<br>}<br><br>print </span><span class="string">&quot;Done! :^)\n\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Which outputs:<br>php -q fork_n_wait.php<br>FORK: Child #1 preparing to nuke...<br>PHP Fatal error:&#xA0; Call to undefined function generate_fatal_error() in ~fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>FORK: Child #2 preparing to nuke...<br>PHP Fatal error:&#xA0; Call to undefined function generate_fatal_error() in ~/fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>FORK: Child #3 preparing to nuke...<br>PHP Fatal error:&#xA0; Call to undefined function generate_fatal_error() in ~/fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>FORK: Child #4 preparing to nuke...<br>PHP Fatal error:&#xA0; Call to undefined function generate_fatal_error() in ~/fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>Done! :^)</span>
</div>
  

#


<div class="phpcode"><span class="html">
I just thought of contributing to this awesome community and hope this can be of use to someone. Although PHP provides threaded options, and multi curl handles that run in parallel, I managed to bash out a solution to run each function as it&apos;s own process for non-threaded versions of PHP.<br><br>Usage:&#xA0; #!/usr/bin/php<br>Usage: php -f /path/to/file<br><br>#!/usr/bin/php<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">fork_process</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">) <br>{<br>&#xA0; &#xA0; </span><span class="default">$shared_memory_monitor </span><span class="keyword">= </span><span class="default">shmop_open</span><span class="keyword">(</span><span class="default">ftok</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">, </span><span class="default">chr</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">)), </span><span class="string">&quot;c&quot;</span><span class="keyword">, </span><span class="default">0644</span><span class="keyword">, </span><span class="default">count</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">]));<br>&#xA0; &#xA0; </span><span class="default">$shared_memory_ids </span><span class="keyword">= (object) array();<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">]); </span><span class="default">$i</span><span class="keyword">++) <br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$shared_memory_ids</span><span class="keyword">-&gt;</span><span class="default">$i </span><span class="keyword">= </span><span class="default">shmop_open</span><span class="keyword">(</span><span class="default">ftok</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">, </span><span class="default">chr</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">)), </span><span class="string">&quot;c&quot;</span><span class="keyword">, </span><span class="default">0644</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;size&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">]); </span><span class="default">$i</span><span class="keyword">++) <br>&#xA0; &#xA0; { <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">(); <br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$pid</span><span class="keyword">) <br>&#xA0; &#xA0; &#xA0; &#xA0; { <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$i</span><span class="keyword">==</span><span class="default">1</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">usleep</span><span class="keyword">(</span><span class="default">100000</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$shared_memory_data </span><span class="keyword">= </span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">][</span><span class="default">$i </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">]();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">shmop_write</span><span class="keyword">(</span><span class="default">$shared_memory_ids</span><span class="keyword">-&gt;</span><span class="default">$i</span><span class="keyword">, </span><span class="default">$shared_memory_data</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">shmop_write</span><span class="keyword">(</span><span class="default">$shared_memory_monitor</span><span class="keyword">, </span><span class="string">&quot;1&quot;</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">-</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; exit(</span><span class="default">$i</span><span class="keyword">); <br>&#xA0; &#xA0; &#xA0; &#xA0; } <br>&#xA0; &#xA0; } <br>&#xA0; &#xA0; while (</span><span class="default">pcntl_waitpid</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">$status</span><span class="keyword">) != -</span><span class="default">1</span><span class="keyword">) <br>&#xA0; &#xA0; { <br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">shmop_read</span><span class="keyword">(</span><span class="default">$shared_memory_monitor</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">count</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">])) == </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;1&quot;</span><span class="keyword">, </span><span class="default">count</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">])))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$shared_memory_ids </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">-</span><span class="default">1</span><span class="keyword">] = </span><span class="default">shmop_read</span><span class="keyword">(</span><span class="default">$shared_memory_ids</span><span class="keyword">-&gt;</span><span class="default">$key</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;size&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">shmop_delete</span><span class="keyword">(</span><span class="default">$shared_memory_ids</span><span class="keyword">-&gt;</span><span class="default">$key</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">shmop_delete</span><span class="keyword">(</span><span class="default">$shared_memory_monitor</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;callback&apos;</span><span class="keyword">](</span><span class="default">$result</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; <br>&#xA0; &#xA0; } <br>}<br><br></span><span class="comment">// Create shared memory block of size 1M for each function.<br></span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;size&apos;</span><span class="keyword">] = </span><span class="default">pow</span><span class="keyword">(</span><span class="default">1024</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">); <br><br></span><span class="comment">// Define 2 functions to run as its own process.<br></span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">][</span><span class="default">0</span><span class="keyword">] = function()<br>{<br>&#xA0; &#xA0; </span><span class="comment">// Whatever you need goes here...<br>&#xA0; &#xA0; // If you need the results, return its value.<br>&#xA0; &#xA0; // Eg: Long running proccess 1<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="string">&apos;Hello &apos;</span><span class="keyword">;<br>};<br></span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;process&apos;</span><span class="keyword">][</span><span class="default">1</span><span class="keyword">] = function()<br>{<br>&#xA0; &#xA0; </span><span class="comment">// Whatever you need goes here...<br>&#xA0; &#xA0; // If you need the results, return its value.<br>&#xA0; &#xA0; // Eg:<br>&#xA0; &#xA0; // Eg: Long running proccess 2<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="string">&apos;World!&apos;</span><span class="keyword">;<br>};<br></span><span class="default">$options</span><span class="keyword">[</span><span class="string">&apos;callback&apos;</span><span class="keyword">] = function(</span><span class="default">$result</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// $results is an array of return values...<br>&#xA0; &#xA0; // $result[0] for $options[&apos;process&apos;][0] &amp;<br>&#xA0; &#xA0; // $result[1] for $options[&apos;process&apos;][1] &amp;<br>&#xA0; &#xA0; // Eg:<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">$result</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">].</span><span class="default">$result</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">].</span><span class="string">&quot;\n&quot;</span><span class="keyword">;&#xA0; &#xA0; <br>};<br></span><span class="default">fork_process</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>If you&apos;d like to get the results back from a webpage, use exec(). Eg: echo exec(&apos;php -f /path/to/file&apos;);<br><br>Continue hacking! :)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Its been easy to fork process with pcntl_fork.. but how can we control or process further once all child processes gets completed.. here is the way we can do that...
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">5</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$pid</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;In child </span><span class="default">$i</span><span class="string">\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; exit(</span><span class="default">$i</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; while (</span><span class="default">pcntl_waitpid</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">$status</span><span class="keyword">) != -</span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">= </span><span class="default">pcntl_wexitstatus</span><span class="keyword">(</span><span class="default">$status</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Child </span><span class="default">$status</span><span class="string"> completed\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The reason for the MySQL &quot;Lost Connection during query&quot; issue when forking is the fact that the child process inherits the parent&apos;s database connection. When the child exits, the connection is closed. If the parent is performing a query at this very moment, it is doing it on an already closed connection, hence the error.
<br>
<br>An easy way to avoid this is to create a new database connection in parent immediately after forking. Don&apos;t forget to force a new connection by passing true in the 4th argument of mysql_connect():
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// Create the MySQL connection
<br></span><span class="default">$db </span><span class="keyword">= </span><span class="default">mysql_connect</span><span class="keyword">(</span><span class="default">$server</span><span class="keyword">, </span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">);
<br>
<br></span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>if ( </span><span class="default">$pid </span><span class="keyword">== -</span><span class="default">1 </span><span class="keyword">) {&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">// Fork failed&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="keyword">exit(</span><span class="default">1</span><span class="keyword">);
<br>} else if ( </span><span class="default">$pid </span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">// We are the parent
<br>&#xA0; &#xA0; // Can no longer use $db because it will be closed by the child
<br>&#xA0; &#xA0; // Instead, make a new MySQL connection for ourselves to work with
<br>&#xA0; &#xA0; </span><span class="default">$db </span><span class="keyword">= </span><span class="default">mysql_connect</span><span class="keyword">(</span><span class="default">$server</span><span class="keyword">, </span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>} else {
<br>&#xA0; &#xA0; </span><span class="comment">// We are the child
<br>&#xA0; &#xA0; // Do something with the inherited connection here
<br>&#xA0; &#xA0; // It will get closed upon exit
<br>&#xA0; &#xA0; </span><span class="keyword">exit(</span><span class="default">0</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>This way, the child will inherit the old connection, will work on it and will close upon exit. The parent won&apos;t care, because it will open a new connection for itself immediately after forking.
<br>
<br>Hope this helps.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to execute some code after your php page has been returned to the user. Try something like this -
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">index</span><span class="keyword">()
<br>{
<br>&#xA0; &#xA0; &#xA0; &#xA0; function </span><span class="default">shutdown</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">posix_kill</span><span class="keyword">(</span><span class="default">posix_getpid</span><span class="keyword">(), </span><span class="default">SIGHUP</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Do some initial processing
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo(</span><span class="string">&quot;Hello World&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Switch over to daemon mode.
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">())
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;&#xA0; &#xA0;&#xA0; </span><span class="comment">// Parent
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ob_end_clean</span><span class="keyword">(); </span><span class="comment">// Discard the output buffer and close
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">STDIN</span><span class="keyword">);&#xA0; </span><span class="comment">// Close all of the standard
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">STDOUT</span><span class="keyword">); </span><span class="comment">// file descriptors as we
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">STDERR</span><span class="keyword">); </span><span class="comment">// are running as a daemon.
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;shutdown&apos;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">posix_setsid</span><span class="keyword">() &lt; </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">())
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;&#xA0; &#xA0;&#xA0; </span><span class="comment">// Parent
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; // Now running as a daemon. This process will even survive
<br>&#xA0; &#xA0; &#xA0; &#xA0; // an apachectl stop.
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;/tmp/sdf123&quot;</span><span class="keyword">, </span><span class="string">&quot;w&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fprintf</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;PID = %s\n&quot;</span><span class="keyword">, </span><span class="default">posix_getpid</span><span class="keyword">());
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; return;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.pcntl-fork.php)

**[⬆ to root](/)**