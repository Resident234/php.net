# register_shutdown_function




<div class="phpcode"><span class="html">
If your script exceeds the maximum execution time, and terminates thusly:<br><br>Fatal error: Maximum execution time of 20 seconds exceeded in - on line 12<br><br>The registered shutdown functions will still be executed.<br><br>I figured it was important that this be made clear!</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to do something with files in function, that registered in register_shutdown_function(), use ABSOLUTE paths to files instead of relative. Because when script processing is complete current working directory chages to ServerRoot (see httpd.conf)</span>
</div>
  

#


<div class="phpcode"><span class="html">
You may get the idea to call debug_backtrace or debug_print_backtrace from inside a shutdown function, to trace where a fatal error occurred. Unfortunately, these functions will not work inside a shutdown function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you register function that needs to be running last (for example, close database connection) - just register another shutdown function from shutdown function:<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">test1</span><span class="keyword">(){<br>&#xA0; </span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;test_last&apos;</span><span class="keyword">);<br>}<br><br>function </span><span class="default">test2</span><span class="keyword">(){</span><span class="comment">/*...*/</span><span class="keyword">}<br>function </span><span class="default">test3</span><span class="keyword">(){</span><span class="comment">/*...*/</span><span class="keyword">}<br>function </span><span class="default">test_last</span><span class="keyword">(){</span><span class="comment">/*...*/</span><span class="keyword">}<br><br></span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;test1&apos;</span><span class="keyword">);<br></span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;test2&apos;</span><span class="keyword">);<br></span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;test3&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>the script will call functions in correct order: test1, test2, test3, test_last</span>
</div>
  

#


<div class="phpcode"><span class="html">
You definitely need to be careful about using relative paths in after the shutdown function has been called, but the current working directory doesn&apos;t (necessarily) get changed to the web server&apos;s ServerRoot - I&apos;ve tested on two different servers and they both have their CWD changed to &apos;/&apos; (which isn&apos;t the ServerRoot).
<br>
<br>This demonstrates the behaviour:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">echocwd</span><span class="keyword">() { echo </span><span class="string">&apos;cwd: &apos;</span><span class="keyword">, </span><span class="default">getcwd</span><span class="keyword">(), </span><span class="string">&quot;\n&quot;</span><span class="keyword">; }
<br>
<br></span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;echocwd&apos;</span><span class="keyword">);
<br></span><span class="default">echocwd</span><span class="keyword">() and exit;
<br></span><span class="default">?&gt;
<br></span>
<br>Outputs:
<br>
<br>cwd: /path/to/my/site/docroot/test
<br>cwd: /
<br>
<br>NB: CLI scripts are unaffected, and keep their CWD as the directory the script was called from.</span>
</div>
  

#


<div class="phpcode"><span class="html">
A lot of useful services may be delegated to this useful trigger.<br>It is very effective because it is executed at the end of the script but before any object destruction, so all instantiations are still alive.<br><br>Here&apos;s a simple shutdown events manager class which allows to manage either functions or static/dynamic methods, with an indefinite number of arguments without using any reflection, availing on a internal handling through func_get_args() and call_user_func_array() specific functions:<br><br><span class="default">&lt;?php<br></span><span class="comment">// managing the shutdown callback events:<br></span><span class="keyword">class </span><span class="default">shutdownScheduler </span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$callbacks</span><span class="keyword">; </span><span class="comment">// array to store user callbacks<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__construct</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">callbacks </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">register_shutdown_function</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;callRegisteredShutdown&apos;</span><span class="keyword">));<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">registerShutdownEvent</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$callback </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if (empty(</span><span class="default">$callback</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="string">&apos;No callback passed to &apos;</span><span class="keyword">.</span><span class="default">__FUNCTION__</span><span class="keyword">.</span><span class="string">&apos; method&apos;</span><span class="keyword">, </span><span class="default">E_USER_ERROR</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">is_callable</span><span class="keyword">(</span><span class="default">$callback</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="string">&apos;Invalid callback passed to the &apos;</span><span class="keyword">.</span><span class="default">__FUNCTION__</span><span class="keyword">.</span><span class="string">&apos; method&apos;</span><span class="keyword">, </span><span class="default">E_USER_ERROR</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">callbacks</span><span class="keyword">[] = </span><span class="default">$callback</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">callRegisteredShutdown</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">callbacks </span><span class="keyword">as </span><span class="default">$arguments</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$callback </span><span class="keyword">= </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$arguments</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func_array</span><span class="keyword">(</span><span class="default">$callback</span><span class="keyword">, </span><span class="default">$arguments</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="comment">// test methods:<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">dynamicTest</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;_REQUEST array is &apos;</span><span class="keyword">.</span><span class="default">count</span><span class="keyword">(</span><span class="default">$_REQUEST</span><span class="keyword">).</span><span class="string">&apos; elements long.&lt;br /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public static function </span><span class="default">staticTest</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;_SERVER array is &apos;</span><span class="keyword">.</span><span class="default">count</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">).</span><span class="string">&apos; elements long.&lt;br /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>A simple application:<br><br><span class="default">&lt;?php<br></span><span class="comment">// a generic function<br></span><span class="keyword">function </span><span class="default">say</span><span class="keyword">(</span><span class="default">$a </span><span class="keyword">= </span><span class="string">&apos;a generic greeting&apos;</span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Saying </span><span class="keyword">{</span><span class="default">$a</span><span class="keyword">}</span><span class="string"> </span><span class="keyword">{</span><span class="default">$b</span><span class="keyword">}</span><span class="string">&lt;br /&gt;&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">$scheduler </span><span class="keyword">= new </span><span class="default">shutdownScheduler</span><span class="keyword">();<br><br></span><span class="comment">// schedule a global scope function:<br></span><span class="default">$scheduler</span><span class="keyword">-&gt;</span><span class="default">registerShutdownEvent</span><span class="keyword">(</span><span class="string">&apos;say&apos;</span><span class="keyword">, </span><span class="string">&apos;hello!&apos;</span><span class="keyword">);<br><br></span><span class="comment">// try to schedule a dyamic method:<br></span><span class="default">$scheduler</span><span class="keyword">-&gt;</span><span class="default">registerShutdownEvent</span><span class="keyword">(array(</span><span class="default">$scheduler</span><span class="keyword">, </span><span class="string">&apos;dynamicTest&apos;</span><span class="keyword">));<br></span><span class="comment">// try with a static call:<br></span><span class="default">$scheduler</span><span class="keyword">-&gt;</span><span class="default">registerShutdownEvent</span><span class="keyword">(</span><span class="string">&apos;scheduler::staticTest&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>It is easy to guess how to extend this example in a more complex context in which user defined functions and methods should be handled according to the priority depending on specific variables.<br><br>Hope it may help somebody.<br>Happy coding!</span>
</div>
  

#


<div class="phpcode"><span class="html">
When using php-fpm, fastcgi_finish_request() should be used instead of register_shutdown_function() and exit()<br><br>For example, under nginx and php-fpm 5.3+, this will make browsers wait 10 seconds to show output:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="string">&quot;You have to wait 10 seconds to see this.&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;shutdown&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; exit;<br>&#xA0; &#xA0; function </span><span class="default">shutdown</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Because exit() doesn&apos;t terminate php-fpm calls immediately.&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>This doesn&apos;t:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="string">&quot;You can see this from the browser immediately.&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">fastcgi_finish_request</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="string">&quot;You can&apos;t see this form the browser.&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
register_shutdown_function seems to be immune to whatever value was set with set_time_limit or the max_execution_time value defined in php.ini. <br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">asdf</span><span class="keyword">() {<br>&#xA0; &#xA0; echo </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>}<br></span><span class="default">register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;asdf&apos;</span><span class="keyword">);<br></span><span class="default">set_time_limit</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br><br>while(</span><span class="default">true</span><span class="keyword">) {}<br></span><span class="default">?&gt;<br></span>The output is three lines.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.register-shutdown-function.php)

**[To root](/README.md)**