# ini_set




<div class="phpcode"><span class="html">
I have experienced on some systems that ini_set() will fail and return a false, when trying to set a setting that was set inside php.ini inside a per-host setting. Beware of this.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When checking for the success of ini_set(), keep in mind that it will return the OLD value upon success - which may have been &quot;0&quot;. Therefore you cannot just compare the return value - you have to check for &quot;identity&quot;:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// This will NOT determine the success of ini_set(), instead<br>// it only tests if the old value had been equivalent to false<br></span><span class="keyword">if ( !</span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&apos;display_errors&apos;</span><span class="keyword">, </span><span class="string">&apos;1&apos; </span><span class="keyword">) ) <br>&#xA0; &#xA0; throw new \</span><span class="default">Exception</span><span class="keyword">( </span><span class="string">&apos;Unable to set display_errors.&apos; </span><span class="keyword">);<br><br></span><span class="comment">// This is the CORRECT way to determine success<br></span><span class="keyword">if ( </span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&apos;display_errors&apos;</span><span class="keyword">, </span><span class="string">&apos;1&apos; </span><span class="keyword">) === </span><span class="default">false </span><span class="keyword">) <br>&#xA0; &#xA0; throw new \</span><span class="default">Exception</span><span class="keyword">( </span><span class="string">&apos;Unable to set display_errors.&apos; </span><span class="keyword">);&#xA0; &#xA0; <br><br></span><span class="default">?&gt;<br></span><br>This explains reported situations where ini_set() &quot;always&quot; seems to fail!</span>
</div>
  

#


<div class="phpcode"><span class="html">
[[[Editors note: Yes, this is very true.&#xA0; Same with 
<br>register_globals, magic_quotes_gpc and others.
<br>]]]
<br>
<br>Many settings, although they do get set, have no influence in your script.... like upload_max_filesize will get set but uploaded files are already passed to your PHP script before the settings are changed.
<br>
<br>Also other settings, set by ini_set(), may be to late because of this (post_max_size etc.).
<br>beware, try settings thru php.ini or .htaccess.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful with setting an output_handler, as you can&apos;t use ini_set() to change it. *sigh*<br><br>In my php.ini I have this for my web pages (and I want it): <br><br>&#xA0; output_handler = ob_gzhandler<br><br>But this causes my command line scripts to not show output until the very end.<br><br>#!/usr/bin/php -q<br><span class="default">&lt;?php<br>ini_set</span><span class="keyword">(</span><span class="string">&apos;output_handler&apos;</span><span class="keyword">, </span><span class="string">&apos;mb_output_handler&apos;</span><span class="keyword">);<br>echo </span><span class="string">&quot;\noutput_handler =&gt; &quot; </span><span class="keyword">. </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&apos;output_handler&apos;</span><span class="keyword">) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>root@# ./myscript.php<br>output_handler =&gt; ob_gzhandler<br><br>Apparently (acording to Richard Lynch):<br><br>&gt; TOO LATE!<br>&gt; The ob_start() has already kicked in by this point.<br>&gt; ob_flush() until there are no more buffers.</span>
</div>
  

#


<div class="phpcode"><span class="html">
set PHP_INI_PERDIR settings in a .htaccess file with &apos;php_flag&apos; like this:<br><br>php_flag register_globals off<br>php_flag magic_quotes_gpc on</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ini-set.php)

**[To root](/README.md)**