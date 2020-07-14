# Supported Protocols and Wrappers



to create a raw tcp listener system i use the following:<br><br>xinetd daemon with config like:<br>service test<br>{<br>        disable      = no<br>        type         = UNLISTED<br>        socket_type  = stream<br>        protocol     = tcp<br>        bind         = 127.0.0.1<br>        port         = 12345<br>        wait         = no<br>        user         = apache<br>        group        = apache<br>        instances    = 10<br>        server       = /usr/local/bin/php<br>        server_args  = -n [your php file here]<br>        only_from    = 127.0.0.1 #gotta love the security#<br>        log_type     = FILE /var/log/phperrors.log<br>        log_on_success += DURATION<br>}<br><br>now use fgets(STDIN) to read the input. Creates connections pretty quick, works like a charm.Writing can be done using the STDOUT, or just echo. Be aware that you&apos;re completely bypassing the webserver and thus certain variables will not be available.  

#

You can use "php://input" to accept and parse "PUT", "DELETE", etc. requests.<br><br>

```
<?php
// Example to parse "PUT" requests 
parse_str(file_get_contents('php://input'), $_PUT);

// The result
print_r($_PUT);
?>
```
<br><br>(very useful for Restful API)  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.php)

**[To root](/README.md)**