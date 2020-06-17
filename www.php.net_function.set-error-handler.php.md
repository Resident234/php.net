# set_error_handler




<div class="phpcode"><span class="html">
By this function alone you can not catch fatal errors, there is a simple work around. Below is part of my error.php file which handles errors and exceptions in the application. Before someone complains I&apos;ll add that I do not care that I am using globals, this file is part of my mini framework and without the &apos;config&apos; variable the application would crash anyways.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * Error handler, passes flow over the exception logger with new ErrorException.<br> */<br></span><span class="keyword">function </span><span class="default">log_error</span><span class="keyword">( </span><span class="default">$num</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">$file</span><span class="keyword">, </span><span class="default">$line</span><span class="keyword">, </span><span class="default">$context </span><span class="keyword">= </span><span class="default">null </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">log_exception</span><span class="keyword">( new </span><span class="default">ErrorException</span><span class="keyword">( </span><span class="default">$str</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">, </span><span class="default">$file</span><span class="keyword">, </span><span class="default">$line </span><span class="keyword">) );<br>}<br><br></span><span class="comment">/**<br> * Uncaught exception handler.<br> */<br></span><span class="keyword">function </span><span class="default">log_exception</span><span class="keyword">( </span><span class="default">Exception $e </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; global </span><span class="default">$config</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if ( </span><span class="default">$config</span><span class="keyword">[</span><span class="string">&quot;debug&quot;</span><span class="keyword">] == </span><span class="default">true </span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;div style=&apos;text-align: center;&apos;&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;h2 style=&apos;color: rgb(190, 50, 50);&apos;&gt;Exception Occured:&lt;/h2&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;table style=&apos;width: 800px; display: inline-block;&apos;&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;tr style=&apos;background-color:rgb(230,230,230);&apos;&gt;&lt;th style=&apos;width: 80px;&apos;&gt;Type&lt;/th&gt;&lt;td&gt;&quot; </span><span class="keyword">. </span><span class="default">get_class</span><span class="keyword">( </span><span class="default">$e </span><span class="keyword">) . </span><span class="string">&quot;&lt;/td&gt;&lt;/tr&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;tr style=&apos;background-color:rgb(240,240,240);&apos;&gt;&lt;th&gt;Message&lt;/th&gt;&lt;td&gt;</span><span class="keyword">{</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">()}</span><span class="string">&lt;/td&gt;&lt;/tr&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;tr style=&apos;background-color:rgb(230,230,230);&apos;&gt;&lt;th&gt;File&lt;/th&gt;&lt;td&gt;</span><span class="keyword">{</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getFile</span><span class="keyword">()}</span><span class="string">&lt;/td&gt;&lt;/tr&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;tr style=&apos;background-color:rgb(240,240,240);&apos;&gt;&lt;th&gt;Line&lt;/th&gt;&lt;td&gt;</span><span class="keyword">{</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getLine</span><span class="keyword">()}</span><span class="string">&lt;/td&gt;&lt;/tr&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;/table&gt;&lt;/div&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;Type: &quot; </span><span class="keyword">. </span><span class="default">get_class</span><span class="keyword">( </span><span class="default">$e </span><span class="keyword">) . </span><span class="string">&quot;; Message: </span><span class="keyword">{</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">()}</span><span class="string">; File: </span><span class="keyword">{</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getFile</span><span class="keyword">()}</span><span class="string">; Line: </span><span class="keyword">{</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getLine</span><span class="keyword">()}</span><span class="string">;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">file_put_contents</span><span class="keyword">( </span><span class="default">$config</span><span class="keyword">[</span><span class="string">&quot;app_dir&quot;</span><span class="keyword">] . </span><span class="string">&quot;/tmp/logs/exceptions.log&quot;</span><span class="keyword">, </span><span class="default">$message </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">, </span><span class="default">FILE_APPEND </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">( </span><span class="string">&quot;Location: </span><span class="keyword">{</span><span class="default">$config</span><span class="keyword">[</span><span class="string">&quot;error_page&quot;</span><span class="keyword">]}</span><span class="string">&quot; </span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; exit();<br>}<br><br></span><span class="comment">/**<br> * Checks for a fatal error, work around for set_error_handler not working on fatal errors.<br> */<br></span><span class="keyword">function </span><span class="default">check_for_fatal</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="default">error_get_last</span><span class="keyword">();<br>&#xA0; &#xA0; if ( </span><span class="default">$error</span><span class="keyword">[</span><span class="string">&quot;type&quot;</span><span class="keyword">] == </span><span class="default">E_ERROR </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">log_error</span><span class="keyword">( </span><span class="default">$error</span><span class="keyword">[</span><span class="string">&quot;type&quot;</span><span class="keyword">], </span><span class="default">$error</span><span class="keyword">[</span><span class="string">&quot;message&quot;</span><span class="keyword">], </span><span class="default">$error</span><span class="keyword">[</span><span class="string">&quot;file&quot;</span><span class="keyword">], </span><span class="default">$error</span><span class="keyword">[</span><span class="string">&quot;line&quot;</span><span class="keyword">] );<br>}<br><br></span><span class="default">register_shutdown_function</span><span class="keyword">( </span><span class="string">&quot;check_for_fatal&quot; </span><span class="keyword">);<br></span><span class="default">set_error_handler</span><span class="keyword">( </span><span class="string">&quot;log_error&quot; </span><span class="keyword">);<br></span><span class="default">set_exception_handler</span><span class="keyword">( </span><span class="string">&quot;log_exception&quot; </span><span class="keyword">);<br></span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&quot;display_errors&quot;</span><span class="keyword">, </span><span class="string">&quot;off&quot; </span><span class="keyword">);<br></span><span class="default">error_reporting</span><span class="keyword">( </span><span class="default">E_ALL </span><span class="keyword">);</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">/**<br> * throw exceptions based on E_* error types<br> */<br></span><span class="default">set_error_handler</span><span class="keyword">(function (</span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">, array </span><span class="default">$err_context</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// error was suppressed with the @-operator<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">0 </span><span class="keyword">=== </span><span class="default">error_reporting</span><span class="keyword">()) { return </span><span class="default">false</span><span class="keyword">;}<br>&#xA0; &#xA0; switch(</span><span class="default">$err_severity</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_ERROR</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new </span><span class="default">ErrorException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_WARNING</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new </span><span class="default">WarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_PARSE</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new </span><span class="default">ParseException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_NOTICE</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">NoticeException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_CORE_ERROR</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">CoreErrorException&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_CORE_WARNING</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">CoreWarningException&#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_COMPILE_ERROR</span><span class="keyword">:&#xA0; &#xA0; &#xA0;&#xA0; throw new </span><span class="default">CompileErrorException&#xA0; &#xA0;&#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_COMPILE_WARNING</span><span class="keyword">:&#xA0; &#xA0;&#xA0; throw new </span><span class="default">CoreWarningException&#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_ERROR</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">UserErrorException&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_WARNING</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">UserWarningException&#xA0; &#xA0; &#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_NOTICE</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new </span><span class="default">UserNoticeException&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_STRICT</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">StrictException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_RECOVERABLE_ERROR</span><span class="keyword">:&#xA0;&#xA0; throw new </span><span class="default">RecoverableErrorException </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_DEPRECATED</span><span class="keyword">:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">DeprecatedException&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_DEPRECATED</span><span class="keyword">:&#xA0; &#xA0;&#xA0; throw new </span><span class="default">UserDeprecatedException&#xA0;&#xA0; </span><span class="keyword">(</span><span class="default">$err_msg</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$err_severity</span><span class="keyword">, </span><span class="default">$err_file</span><span class="keyword">, </span><span class="default">$err_line</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>});<br><br>class </span><span class="default">WarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">ParseException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">NoticeException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">CoreErrorException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">CoreWarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">CompileErrorException&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">CompileWarningException&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">UserErrorException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">UserWarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">UserNoticeException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">StrictException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">RecoverableErrorException&#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">DeprecatedException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}<br>class </span><span class="default">UserDeprecatedException&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">extends </span><span class="default">ErrorException </span><span class="keyword">{}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">/**<br> * Used for logging all php notices,warings and etc in a file when error reporting<br> * is set and display_errors is off<br> * @uses used in prod env for logging all type of error of php code in a file for further debugging<br> * and code performance<br> * @author Aditya Mehrotra&lt;aditycse@gmail.com&gt;<br> */<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&quot;display_errors&quot;</span><span class="keyword">, </span><span class="string">&quot;off&quot;</span><span class="keyword">);<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ERROR_LOG_FILE&apos;</span><span class="keyword">, </span><span class="string">&apos;/var/www/error.log&apos;</span><span class="keyword">);<br><br></span><span class="comment">/**<br> * Custom error handler<br> * @param integer $code<br> * @param string $description<br> * @param string $file<br> * @param interger $line<br> * @param mixed $context<br> * @return boolean<br> */<br></span><span class="keyword">function </span><span class="default">handleError</span><span class="keyword">(</span><span class="default">$code</span><span class="keyword">, </span><span class="default">$description</span><span class="keyword">, </span><span class="default">$file </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">, </span><span class="default">$line </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">, </span><span class="default">$context </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$displayErrors </span><span class="keyword">= </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&quot;display_errors&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$displayErrors </span><span class="keyword">= </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$displayErrors</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">error_reporting</span><span class="keyword">() === </span><span class="default">0 </span><span class="keyword">|| </span><span class="default">$displayErrors </span><span class="keyword">=== </span><span class="string">&quot;on&quot;</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; list(</span><span class="default">$error</span><span class="keyword">, </span><span class="default">$log</span><span class="keyword">) = </span><span class="default">mapErrorCode</span><span class="keyword">(</span><span class="default">$code</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;level&apos; </span><span class="keyword">=&gt; </span><span class="default">$log</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;code&apos; </span><span class="keyword">=&gt; </span><span class="default">$code</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;error&apos; </span><span class="keyword">=&gt; </span><span class="default">$error</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;description&apos; </span><span class="keyword">=&gt; </span><span class="default">$description</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;file&apos; </span><span class="keyword">=&gt; </span><span class="default">$file</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;line&apos; </span><span class="keyword">=&gt; </span><span class="default">$line</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;context&apos; </span><span class="keyword">=&gt; </span><span class="default">$context</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;path&apos; </span><span class="keyword">=&gt; </span><span class="default">$file</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;message&apos; </span><span class="keyword">=&gt; </span><span class="default">$error </span><span class="keyword">. </span><span class="string">&apos; (&apos; </span><span class="keyword">. </span><span class="default">$code </span><span class="keyword">. </span><span class="string">&apos;): &apos; </span><span class="keyword">. </span><span class="default">$description </span><span class="keyword">. </span><span class="string">&apos; in [&apos; </span><span class="keyword">. </span><span class="default">$file </span><span class="keyword">. </span><span class="string">&apos;, line &apos; </span><span class="keyword">. </span><span class="default">$line </span><span class="keyword">. </span><span class="string">&apos;]&apos;<br>&#xA0; &#xA0; </span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">fileLog</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>}<br><br></span><span class="comment">/**<br> * This method is used to write data in file<br> * @param mixed $logData<br> * @param string $fileName<br> * @return boolean<br> */<br></span><span class="keyword">function </span><span class="default">fileLog</span><span class="keyword">(</span><span class="default">$logData</span><span class="keyword">, </span><span class="default">$fileName </span><span class="keyword">= </span><span class="default">ERROR_LOG_FILE</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$fh </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$fileName</span><span class="keyword">, </span><span class="string">&apos;a+&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$logData</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$logData </span><span class="keyword">= </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$logData</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">= </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, </span><span class="default">$logData</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">);<br>&#xA0; &#xA0; return (</span><span class="default">$status</span><span class="keyword">) ? </span><span class="default">true </span><span class="keyword">: </span><span class="default">false</span><span class="keyword">;<br>}<br><br></span><span class="comment">/**<br> * Map an error code into an Error word, and log location.<br> *<br> * @param int $code Error code to map<br> * @return array Array of error word, and log location.<br> */<br></span><span class="keyword">function </span><span class="default">mapErrorCode</span><span class="keyword">(</span><span class="default">$code</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="default">$log </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; switch (</span><span class="default">$code</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_PARSE</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_ERROR</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_CORE_ERROR</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_COMPILE_ERROR</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_ERROR</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="string">&apos;Fatal Error&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$log </span><span class="keyword">= </span><span class="default">LOG_ERR</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_WARNING</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_WARNING</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_COMPILE_WARNING</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_RECOVERABLE_ERROR</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="string">&apos;Warning&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$log </span><span class="keyword">= </span><span class="default">LOG_WARNING</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_NOTICE</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_NOTICE</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="string">&apos;Notice&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$log </span><span class="keyword">= </span><span class="default">LOG_NOTICE</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_STRICT</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="string">&apos;Strict&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$log </span><span class="keyword">= </span><span class="default">LOG_NOTICE</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_DEPRECATED</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_DEPRECATED</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="string">&apos;Deprecated&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$log </span><span class="keyword">= </span><span class="default">LOG_NOTICE</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; default :<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return array(</span><span class="default">$error</span><span class="keyword">, </span><span class="default">$log</span><span class="keyword">);<br>}<br><br></span><span class="comment">//calling custom error handler<br></span><span class="default">set_error_handler</span><span class="keyword">(</span><span class="string">&quot;handleError&quot;</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$arra</span><span class="keyword">); </span><span class="comment">//undefined variable<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$dssdfdfgg</span><span class="keyword">); </span><span class="comment">//undefined variable<br></span><span class="keyword">include_once </span><span class="string">&apos;file.php&apos;</span><span class="keyword">; </span><span class="comment">//No such file or directory<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.set-error-handler.php)

**[To root](/README.md)**