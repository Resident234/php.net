# Description of core php.ini directives




<div class="phpcode"><span class="html">
Starting with PHP 4.4.0 (at least PHP version 4.3.10 did have old, documented behaviour) interpretation of value of &quot;session.save_path&quot; did change in conjunction with &quot;save_mode&quot; and &quot;open_basedir&quot; enabled.<br><br>Documented ( <a href="http://de.php.net/manual/en/ref.session.php#ini.session.save-path" rel="nofollow" target="_blank">http://de.php.net/manual/en/ref.session.php#ini.session.save-path</a> ):<br>&#xA0; Values of &quot;session.save_path&quot; should or may be&#xA0; **without**&#xA0; ending slash.<br>&#xA0; For instance:<br><span class="default">&lt;?php<br>&#xA0; </span><span class="comment">// Valid only&#xA0; *before* PHP 4.4.0:<br>&#xA0; </span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&quot;session.save_path&quot;</span><span class="keyword">, </span><span class="string">&quot;/var/httpd/kunde/phptmp&quot; </span><span class="keyword">);<br></span><span class="default">?&gt;</span> will mean:<br>&#xA0; The directory &quot;/var/httpd/kunde/phptmp/&quot; will be used to write data and therefore must be writable by the web server.<br><br>Starting with PHP 4.4.0 the server complains that &quot;/var/httpd/kunde/&quot; is not writable.<br>Solution: Add an ending slash in call of ini_set (or probably whereever you set &quot;session.save_path&quot;), e.g.:<br><span class="default">&lt;?php<br>&#xA0; </span><span class="comment">// Note the slash on &quot;.....phptmp/&quot;:<br>&#xA0; </span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&quot;session.save_path&quot;</span><span class="keyword">, </span><span class="string">&quot;/var/httpd/kunde/phptmp/&quot; </span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Hope, that does help someone.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ini.core.php)

**[To root](/README.md)**