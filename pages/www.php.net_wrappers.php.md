# Supported Protocols and Wrappers





to create a raw tcp listener system i use the following:

xinetd daemon with config like:
service test
{
&#xA0; &#xA0; &#xA0; &#xA0; disable&#xA0; &#xA0; &#xA0; = no
&#xA0; &#xA0; &#xA0; &#xA0; type&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = UNLISTED
&#xA0; &#xA0; &#xA0; &#xA0; socket_type&#xA0; = stream
&#xA0; &#xA0; &#xA0; &#xA0; protocol&#xA0; &#xA0;&#xA0; = tcp
&#xA0; &#xA0; &#xA0; &#xA0; bind&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = 127.0.0.1
&#xA0; &#xA0; &#xA0; &#xA0; port&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = 12345
&#xA0; &#xA0; &#xA0; &#xA0; wait&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = no
&#xA0; &#xA0; &#xA0; &#xA0; user&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = apache
&#xA0; &#xA0; &#xA0; &#xA0; group&#xA0; &#xA0; &#xA0; &#xA0; = apache
&#xA0; &#xA0; &#xA0; &#xA0; instances&#xA0; &#xA0; = 10
&#xA0; &#xA0; &#xA0; &#xA0; server&#xA0; &#xA0; &#xA0;&#xA0; = /usr/local/bin/php
&#xA0; &#xA0; &#xA0; &#xA0; server_args&#xA0; = -n [your php file here]
&#xA0; &#xA0; &#xA0; &#xA0; only_from&#xA0; &#xA0; = 127.0.0.1 #gotta love the security#
&#xA0; &#xA0; &#xA0; &#xA0; log_type&#xA0; &#xA0;&#xA0; = FILE /var/log/phperrors.log
&#xA0; &#xA0; &#xA0; &#xA0; log_on_success += DURATION
}

now use fgets(STDIN) to read the input. Creates connections pretty quick, works like a charm.Writing can be done using the STDOUT, or just echo. Be aware that you&apos;re completely bypassing the webserver and thus certain variables will not be available.

  

#



You can use &quot;php://input&quot; to accept and parse &quot;PUT&quot;, &quot;DELETE&quot;, etc. requests.



```
<?php
// Example to parse &quot;PUT&quot; requests 
parse_str(file_get_contents(&apos;php://input&apos;), $_PUT);

// The result
print_r($_PUT);
?>
```


(very useful for Restful API)

  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.php)

**[To root](/README.md)**