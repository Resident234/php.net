# session_save_path




<div class="phpcode"><span class="html">
I made a folder next to the public html folder and placed these lines at the very first point in index.php<br><br>Location of session folder:<br><br>/domains/account/session<br><br>location of index.php<br><br>/domains/account/public_html/index.php<br><br>What I placed in index.php at line 0:<br><br><span class="default">&lt;?php <br>ini_set</span><span class="keyword">(</span><span class="string">&apos;session.save_path&apos;</span><span class="keyword">,</span><span class="default">realpath</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;DOCUMENT_ROOT&apos;</span><span class="keyword">]) . </span><span class="string">&apos;/../session&apos;</span><span class="keyword">));<br></span><span class="default">session_start</span><span class="keyword">();<br><br></span><span class="default">This is the only solution that worked </span><span class="keyword">for </span><span class="default">me</span><span class="keyword">. </span><span class="default">Hope this helps someone</span><span class="keyword">.</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Debian does not use the default garbage collector for sessions. Instead, it sets session.gc_probability to zero and it runs a cron job to clean up old session data in the default directory.<br><br>As a result, if your site sets a custom location with session_save_path() you also need to set a value for session.gc_probability, e.g.:<br><br><span class="default">&lt;?php<br>session_save_path</span><span class="keyword">(</span><span class="string">&apos;/home/example.com/sessions&apos;</span><span class="keyword">);<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;session.gc_probability&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Otherwise, old files in &apos;/home/example.com/sessions&apos; will never get removed!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Session on clustered web servers !<br><br>We had problem in PHP session handling with 2 web server cluster. Problem was one servers session data was not available in other server.<br><br>So I made a simple configuration in both server php.ini file. Changed session.save_path default value to shared folder on both servers (/mnt/session/).<br><br>It works for me. :)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-save-path.php)

**[To root](/README.md)**