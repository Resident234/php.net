# Error Control Operators




<div class="phpcode"><span class="html">
This operator is affectionately known by veteran phpers as the stfu operator.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I was confused as to what the @ symbol actually does, and after a few experiments have concluded the following:<br><br>* the error handler that is set gets called regardless of what level the error reporting is set on, or whether the statement is preceeded with @<br><br>* it is up to the error handler to impart some meaning on the different error levels. You could make your custom error handler echo all errors, even if error reporting is set to NONE.<br><br>* so what does the @ operator do? It temporarily sets the error reporting level to 0 for that line. If that line triggers an error, the error handler will still be called, but it will be called with an error level of 0<br><br>Hope this helps someone</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be aware of using error control operator in statements before include() like this:<br><br><span class="default">&lt;?PHP<br><br></span><span class="keyword">(@include(</span><span class="string">&quot;file.php&quot;</span><span class="keyword">))<br> OR die(</span><span class="string">&quot;Could not find file.php!&quot;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>This cause, that error reporting level is set to zero also for the included file. So if there are some errors in the included file, they will be not displayed.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re wondering what the performance impact of using the @ operator is, consider this example.&#xA0; Here, the second script (using the @ operator) takes 1.75x as long to execute...almost double the time of the first script.
<br>
<br>So while yes, there is some overhead, per iteration, we see that the @ operator added only .005 ms per call.&#xA0; Not reason enough, imho, to avoid using the @ operator.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">x</span><span class="keyword">() { }
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">1000000</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) { </span><span class="default">x</span><span class="keyword">(); }
<br></span><span class="default">?&gt;
<br></span>
<br>real&#xA0; &#xA0; 0m7.617s
<br>user&#xA0; &#xA0; 0m6.788s
<br>sys&#xA0; &#xA0; 0m0.792s
<br>
<br>vs
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">x</span><span class="keyword">() { }
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">1000000</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) { @</span><span class="default">x</span><span class="keyword">(); }
<br></span><span class="default">?&gt;
<br></span>
<br>real&#xA0; &#xA0; 0m13.333s
<br>user&#xA0; &#xA0; 0m12.437s
<br>sys&#xA0; &#xA0; 0m0.836s</span>
</div>
  

#


<div class="phpcode"><span class="html">
Error suppression should be avoided if possible as it doesn&apos;t just suppress the error that you are trying to stop, but will also suppress errors that you didn&apos;t predict would ever occur. This will make debugging a nightmare.<br><br>It is far better to test for the condition that you know will cause an error before preceding to run the code. This way only the error that you know about will be suppressed and not all future errors associated with that piece of code.<br><br>There may be a good reason for using outright error suppression in favor of the method I have suggested, however in the many years I&apos;ve spent programming web apps I&apos;ve yet to come across a situation where it was a good solution. The examples given on this manual page are certainly not situations where the error control operator should be used.</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is no reason to NOT use something just because &quot;it can be misused&quot;.&#xA0; You could as well say &quot;unlink is evil, you can delete files with it so don&apos;t ever use unlink&quot;.<br><br>It&apos;s a valid point that the @ operator hides all errors - so my rule of thumb is: use it only if you&apos;re aware of all possible errors your expression can throw AND you consider all of them irrelevant.<br><br>A simple example is<br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; $x </span><span class="keyword">= @</span><span class="default">$a</span><span class="keyword">[</span><span class="string">&quot;name&quot;</span><span class="keyword">];<br><br></span><span class="default">?&gt;<br></span>There are only 2 possible problems here: a missing variable or a missing index.&#xA0; If you&apos;re sure you&apos;re fine with both cases, you&apos;re good to go.&#xA0; And again: suppressing errors is not a crime.&#xA0; Not knowing when it&apos;s safe to suppress them is definitely worse.</span>
</div>
  

#


<div class="phpcode"><span class="html">
After some time investigating as to why I was still getting errors that were supposed to be suppressed with @ I found the following.<br><br>1. If you have set your own default error handler then the error still gets sent to the error handler regardless of the @ sign.<br><br>2. As mentioned below the @ suppression only changes the error level for that call. This is not to say that in your error handler you can check the given $errno for a value of 0 as the $errno will still refer to the TYPE(not the error level) of error e.g. E_WARNING or E_ERROR etc<br><br>3. The @ only changes the rumtime error reporting level just for that one call to 0. This means inside your custom error handler you can check the current runtime error_reporting level using error_reporting() (note that one must NOT pass any parameter to this function if you want to get the current value) and if its zero then you know that it has been suppressed.<br><span class="default">&lt;?php<br></span><span class="comment">// Custom error handler<br></span><span class="keyword">function </span><span class="default">myErrorHandler</span><span class="keyword">(</span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">, </span><span class="default">$errfile</span><span class="keyword">, </span><span class="default">$errline</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if ( </span><span class="default">0 </span><span class="keyword">== </span><span class="default">error_reporting </span><span class="keyword">() ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Error reporting is currently turned off or suppressed with @<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="comment">// Do your normal custom error reporting here<br></span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>For more info on setting a custom error handler see: <a href="http://php.net/manual/en/function.set-error-handler.php" rel="nofollow" target="_blank">http://php.net/manual/en/function.set-error-handler.php</a><br>For more info on error_reporting see: <a href="http://www.php.net/manual/en/function.error-reporting.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.error-reporting.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
To suppress errors for a new class/object:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Tested: PHP 5.1.2 ~ 2006-10-13<br><br>// Typical Example<br></span><span class="default">$var </span><span class="keyword">= @</span><span class="default">some_function</span><span class="keyword">();<br><br></span><span class="comment">// Class/Object Example<br></span><span class="default">$var </span><span class="keyword">= @new </span><span class="default">some_class</span><span class="keyword">();<br><br></span><span class="comment">// Does NOT Work!<br>//$var = new @some_class(); // syntax error<br></span><span class="default">?&gt;<br></span><br>I found this most useful when connecting to a<br>database, where i wanted to control the errors<br>and warnings displayed to the client, while still<br>using the class style of access.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be aware that using @ is dog-slow, as PHP incurs overhead to suppressing errors in this way. It&apos;s a trade-off between speed and convenience.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.errorcontrol.php)

**[To root](/)**