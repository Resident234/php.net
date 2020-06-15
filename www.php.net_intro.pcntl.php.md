# Introduction




<div class="phpcode"><span class="html">
The following statement left me searching for answers for about a day before I finally clued in:<br><br>&quot;Process Control should not be enabled within a web server environment and unexpected results may happen if any Process Control functions are used within a web server environment.&quot;<br><br>At least for PHP 5.3.8 which I am using, and who knows how far back, it&apos;s not a matter of &quot;should not&quot;, it&apos;s &quot;can not&quot;. Even though I have compiled in PCNTL with --enable-pcntl, it turns out that it only compiles in to the CLI version of PHP, not the Apache module. As a result, I spent many hours trying to track down why function_exists(&apos;pcntl_fork&apos;) was returning false even though it compiled correctly. It turns out it returns true just fine from the CLI, and only returns false for HTTP requests. The same is true of ALL of the pcntl_*() functions.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Actually it makes perfect sense why process control features are not supported for the Apache module. The Apache HTTP server is the chief process. It invokes the PHP module when steered to PHP by the resource requested (e.g. <a href="http://foo.php" rel="nofollow" target="_blank">http://foo.php</a>) It invokes the PHP module, typically on a new thread or a pooled thread. The PHP module then runs your script, but Apache server is still the owning process.<br><br>In this execution model, the job of your PHP script is generally to go about its business as fast as possible and return. This allows the Apache daemon to do something else useful with the thread it let you borrow. Yes, some scripts take longer to do their duty than others, but blocking the thread for extended periods is usually frowned upon.<br><br>If your script was allowed to mess with the signal handlers of the running process, it would be messing with the Apache daemon itself! That daemon has already installed signal handlers for its own use. It is just plain sense not to allow the process control operations in this context.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/intro.pcntl.php)

**[To root](/README.md)**