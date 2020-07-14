# Hiding PHP



So far I haven&apos;t seen a working rewriter of /foo/bar into /foo/bar.php, so I created my own. It does work in top-level directory AND subdirectories and it doesn&apos;t need hardcoding the RewriteBase.<br><br>.htaccess:<br><br>RewriteEngine on<br><br># Rewrite /foo/bar to /foo/bar.php<br>RewriteRule ^([^.?]+)$ %{REQUEST_URI}.php [L]<br><br># Return 404 if original request is /foo/bar.php<br>RewriteCond %{THE_REQUEST} "^[^ ]* .*?\.php[? ].*$"<br>RewriteRule .* - [L,R=404]<br><br># NOTE! FOR APACHE ON WINDOWS: Add [NC] to RewriteCond like this:<br># RewriteCond %{THE_REQUEST} "^[^ ]* .*?\.php[? ].*$" [NC]  

#

The session name defaults to PHPSESSID.  This is used as the name of the session cookie that is sent to the user&apos;s web browser / client. (Example: PHPSESSID=kqjqper294faui343o98ts8k77).<br><br>To hide this, call session_name() with the $name parameter set to a generic name, before calling session_start().  Example:<br><br>session_name("id");<br>session_start();<br><br>Cheers.  

#

[Official documentation page](https://www.php.net/manual/en/security.hiding.php)

**[To root](/README.md)**