# getenv




<div class="phpcode"><span class="html">
Contrary to what eng.mrkto.com said, getenv() isn&apos;t always case-insensitive. On Linux it is not:<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">getenv</span><span class="keyword">(</span><span class="string">&apos;path&apos;</span><span class="keyword">)); </span><span class="comment">// bool(false)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">getenv</span><span class="keyword">(</span><span class="string">&apos;Path&apos;</span><span class="keyword">)); </span><span class="comment">// bool(false)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">getenv</span><span class="keyword">(</span><span class="string">&apos;PATH&apos;</span><span class="keyword">)); </span><span class="comment">// string(13) &quot;/usr/bin:/bin&quot;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function is useful (compared to $_SERVER, $_ENV) because it searches $varname key in those array case-insensitive manner.<br>For example on Windows $_SERVER[&apos;Path&apos;] is like you see Capitalized, not &apos;PATH&apos; as you expected.<br>So just: <span class="default">&lt;?php getenv</span><span class="keyword">(</span><span class="string">&apos;path&apos;</span><span class="keyword">) </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
All of the notes and examples so far have been strictly CGI.<br>It should not be understated the usefulness of getenv()/putenv() in CLI as well.<br><br>You can pass a number of variables to a CLI script via environment variables, either in Unix/Linux bash/sh with the &quot;VAR=&apos;foo&apos;; export $VAR&quot; paradigm, or in Windows with the &quot;set VAR=&apos;foo&apos;&quot; paradigm. (Csh users, you&apos;re on your own!) getenv(&quot;VAR&quot;) will retrieve that value from the environment.<br><br>We have a system by which we include a file full of putenv() statements storing configuration values that can apply to many different CLI PHP programs. But if we want to override these values, we can use the shell&apos;s (or calling application, such as ant) environment variable setting method to do so.<br><br>This saves us from having to manage an unmanageable amount of one-off configuration changes per execution via command line arguments; instead we just set the appropriate env var first.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.getenv.php)

**[To root](/)**