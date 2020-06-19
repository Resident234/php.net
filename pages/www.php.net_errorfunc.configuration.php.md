# Runtime Configuration




<div class="phpcode"><span class="html">
Using 
<br><span class="default">&lt;?php ini_set</span><span class="keyword">(</span><span class="string">&apos;display_errors&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">); </span><span class="default">?&gt;</span> 
<br>at the top of your script will not catch any parse errors. A missing &quot;)&quot; or &quot;;&quot; will still lead to a blank page.
<br>
<br>This is because the entire script is parsed before any of it is executed. If you are unable to change php.ini and set
<br>
<br>display_errors On
<br>
<br>then there is a possible solution suggested under error_reporting:
<br>
<br><span class="default">&lt;?php
<br> error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);
<br> </span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&quot;display_errors&quot;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br> include(</span><span class="string">&quot;file_with_errors.php&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>
<br>[Modified by moderator]
<br>
<br>You should also consider setting error_reporting = -1 in your php.ini and display_errors = On if you are in development mode to see all fatal/parse errors or set error_log to your desired file to log errors instead of display_errors in production (this requires log_errors to be turned on).</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you set the error_log directive to a relative path, it is a path relative to the document root rather than php&apos;s containing folder.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/errorfunc.configuration.php)

**[To root](/README.md)**