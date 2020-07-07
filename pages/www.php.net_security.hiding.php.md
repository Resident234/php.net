# Hiding PHP





So far I haven&apos;t seen a working rewriter of /foo/bar into /foo/bar.php, so I created my own. It does work in top-level directory AND subdirectories and it doesn&apos;t need hardcoding the RewriteBase.

.htaccess:

RewriteEngine on

# Rewrite /foo/bar to /foo/bar.php
RewriteRule ^([^.?]+)$ %{REQUEST_URI}.php [L]

# Return 404 if original request is /foo/bar.php
RewriteCond %{THE_REQUEST} &quot;^[^ ]* .*?\.php[? ].*$&quot;
RewriteRule .* - [L,R=404]

# NOTE! FOR APACHE ON WINDOWS: Add [NC] to RewriteCond like this:
# RewriteCond %{THE_REQUEST} &quot;^[^ ]* .*?\.php[? ].*$&quot; [NC]

  

#



The session name defaults to PHPSESSID.&#xA0; This is used as the name of the session cookie that is sent to the user&apos;s web browser / client. (Example: PHPSESSID=kqjqper294faui343o98ts8k77).

To hide this, call session_name() with the $name parameter set to a generic name, before calling session_start().&#xA0; Example:

session_name(&quot;id&quot;);
session_start();

Cheers.

  

#

[Official documentation page](https://www.php.net/manual/en/security.hiding.php)

**[To root](/README.md)**