# Runtime Configuration



On Ubuntu 13.04, not sure of the other Distros. <br><br>If you simply uncomment the default:<br><br>sendmail_path = "sendmail -t -i"<br><br>Your mail() functions will all fail.  This is because, you should place the FULL PATH (i.e.  /usr/sbin/sendmail -t -i ) <br><br>The documentation states PHP tries it&apos;s best to find the correct sendmail path, but it clearly failed for me.<br><br>So, always enter in the FULLPATH to sendmail or you may get unexpected failing results.<br><br>As a secondary note:  Those that just want to ENFORCE the -f parameter, you can do so in php.ini using: <br><br>mail.force_extra_parameters = -fdo_not_reply@domain.tld<br><br>You can leave the sendmail path commented out, it will still use the defaults  (under UNIX  -t -i options which if you look them up are very important to have set)....<br><br>But, now there is no way to change this, even with the 5th argument of the mail() function.  -f is important, because if NOT set, will be set to which ever user the PHP script is running under, and you may not want that.<br><br>Also, -f  sets the Return-Path:  header which is used as the Bounce address, if errors occur, so you can process them.  You you can not set Return-Path: in mail() headers for some reason... you could before.  Now you have to use the -f option.  

#

[Official documentation page](https://www.php.net/manual/en/mail.configuration.php)

**[To root](/README.md)**