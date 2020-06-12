# Supported Protocols and Wrappers




<div class="phpcode"><span class="html">
to create a raw tcp listener system i use the following:<br><br>xinetd daemon with config like:<br>service test<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; disable&#xA0; &#xA0; &#xA0; = no<br>&#xA0; &#xA0; &#xA0; &#xA0; type&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = UNLISTED<br>&#xA0; &#xA0; &#xA0; &#xA0; socket_type&#xA0; = stream<br>&#xA0; &#xA0; &#xA0; &#xA0; protocol&#xA0; &#xA0;&#xA0; = tcp<br>&#xA0; &#xA0; &#xA0; &#xA0; bind&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = 127.0.0.1<br>&#xA0; &#xA0; &#xA0; &#xA0; port&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = 12345<br>&#xA0; &#xA0; &#xA0; &#xA0; wait&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = no<br>&#xA0; &#xA0; &#xA0; &#xA0; user&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = apache<br>&#xA0; &#xA0; &#xA0; &#xA0; group&#xA0; &#xA0; &#xA0; &#xA0; = apache<br>&#xA0; &#xA0; &#xA0; &#xA0; instances&#xA0; &#xA0; = 10<br>&#xA0; &#xA0; &#xA0; &#xA0; server&#xA0; &#xA0; &#xA0;&#xA0; = /usr/local/bin/php<br>&#xA0; &#xA0; &#xA0; &#xA0; server_args&#xA0; = -n [your php file here]<br>&#xA0; &#xA0; &#xA0; &#xA0; only_from&#xA0; &#xA0; = 127.0.0.1 #gotta love the security#<br>&#xA0; &#xA0; &#xA0; &#xA0; log_type&#xA0; &#xA0;&#xA0; = FILE /var/log/phperrors.log<br>&#xA0; &#xA0; &#xA0; &#xA0; log_on_success += DURATION<br>}<br><br>now use fgets(STDIN) to read the input. Creates connections pretty quick, works like a charm.Writing can be done using the STDOUT, or just echo. Be aware that you&apos;re completely bypassing the webserver and thus certain variables will not be available.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use &quot;php://input&quot; to accept and parse &quot;PUT&quot;, &quot;DELETE&quot;, etc. requests.<br><br><span class="default">&lt;?php<br></span><span class="comment">// Example to parse &quot;PUT&quot; requests <br></span><span class="default">parse_str</span><span class="keyword">(</span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;php://input&apos;</span><span class="keyword">), </span><span class="default">$_PUT</span><span class="keyword">);<br><br></span><span class="comment">// The result<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$_PUT</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>(very useful for Restful API)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.php)

**[To root](/)**