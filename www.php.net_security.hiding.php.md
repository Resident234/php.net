# Hiding PHP




<div class="phpcode"><span class="html">
So far I haven&apos;t seen a working rewriter of /foo/bar into /foo/bar.php, so I created my own. It does work in top-level directory AND subdirectories and it doesn&apos;t need hardcoding the RewriteBase.<br><br>.htaccess:<br><br>RewriteEngine on<br><br># Rewrite /foo/bar to /foo/bar.php<br>RewriteRule ^([^.?]+)$ %{REQUEST_URI}.php [L]<br><br># Return 404 if original request is /foo/bar.php<br>RewriteCond %{THE_REQUEST} &quot;^[^ ]* .*?\.php[? ].*$&quot;<br>RewriteRule .* - [L,R=404]<br><br># NOTE! FOR APACHE ON WINDOWS: Add [NC] to RewriteCond like this:<br># RewriteCond %{THE_REQUEST} &quot;^[^ ]* .*?\.php[? ].*$&quot; [NC]</span>
</div>
  

#


<div class="phpcode"><span class="html">
The session name defaults to PHPSESSID.&#xA0; This is used as the name of the session cookie that is sent to the user&apos;s web browser / client. (Example: PHPSESSID=kqjqper294faui343o98ts8k77).<br><br>To hide this, call session_name() with the $name parameter set to a generic name, before calling session_start().&#xA0; Example:<br><br>session_name(&quot;id&quot;);<br>session_start();<br><br>Cheers.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.hiding.php)

**[⬆ to root](/)**