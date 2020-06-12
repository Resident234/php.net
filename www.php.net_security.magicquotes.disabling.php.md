# Disabling Magic Quotes




<div class="phpcode"><span class="html">
I have discovered that my host doesn&apos;t like either of the following directives in the .htaccess file:<br><br>php_flag magic_quotes_gpc Off<br>php_value magic_quotes_gpc Off<br><br>However, there is another way to disable this setting even if you don&apos;t have access to the server configuration - you can put a php.ini file in the directory where your scripts are with the directive:<br><br>magic_quotes_gpc = Off<br><br>However, these does not propogate unlike&#xA0; .htaccess rules, so if you launch from a sub-directory, you need the php.ini file in each directory you have as script entry points.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a couple tips when using scripts on different (often shared) hosts, where ini_set doesn&apos;t work and php directives in .htaccess causes a 500 Internal Server Error.<br><br>Firstly, copy the server&apos;s php.ini file to your domain&apos;s web-root folder. To find the correct paths, use phpinfo() and look for &quot;Configuration File (php.ini) Path&quot; and &quot;DOCUMENT_ROOT&quot;<br><br>It&apos;s unlikely you&apos;ll have access to the php.ini via FTP, so instead run a script with a simple copy command (obviously inserting your paths):<br><br>exec(&quot;cp /usr/local/php/etc/php.ini /home/LinuxPackage/public_html/php.ini);<br><br>Edit the now-accessible php.ini file, and add settings like &apos;magic_quotes_gpc = off&apos; at the bottom (regardless of whether they&apos;ve been set earlier in the file). I also set:<br><br>[PHP]<br>max_execution_time = 60<br>max_input_time = 90<br>memory_limit = 64M<br>post_max_size = 32M<br>upload_max_filesize = 31M<br>magic_quotes_gpc = Off<br><br>Finally add the below line to your web-root htaccess file, to make the local php.ini the web-root default (so you don&apos;t need a copy in every script sub-folder):<br><br>SetEnv PHPRC /home/LinuxPackage/public_html/php.ini<br><br>Hope that helps a few people save some time!<br><br>Mike.<br><br>P.S. Using the new php_ini_loaded_file() function the whole lot could be done in three lines:<br><br>exec(&quot;cp &quot; . php_ini_loaded_file() . &quot; &quot; . $_SERVER[&apos;DOCUMENT_ROOT&apos;] . &quot;/php.ini&quot;);<br>fwrite(fopen(&quot;{$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/php.ini&quot;, &apos;a&apos;), PHP_EOL . &apos;[PHP]&apos; . PHP_EOL . &quot;magic_quotes_gpc = Off&quot; . PHP_EOL);<br>fwrite(fopen(&quot;{$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/.htaccess&quot;, &apos;a&apos;), PHP_EOL . &quot;SetEnv PHPRC {$_SERVER[&apos;DOCUMENT_ROOT&apos;]}/php.ini&quot; . PHP_EOL);</span>
</div>
  

#


<div class="phpcode"><span class="html">
A php5 way:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">get_magic_quotes_gpc</span><span class="keyword">()) {<br>&#xA0; &#xA0; function </span><span class="default">stripslashes_gpc</span><span class="keyword">(&amp;</span><span class="default">$value</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">stripslashes</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">array_walk_recursive</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">, </span><span class="string">&apos;stripslashes_gpc&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_walk_recursive</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">, </span><span class="string">&apos;stripslashes_gpc&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_walk_recursive</span><span class="keyword">(</span><span class="default">$_COOKIE</span><span class="keyword">, </span><span class="string">&apos;stripslashes_gpc&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_walk_recursive</span><span class="keyword">(</span><span class="default">$_REQUEST</span><span class="keyword">, </span><span class="string">&apos;stripslashes_gpc&apos;</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.magicquotes.disabling.php)

**[To root](/)**