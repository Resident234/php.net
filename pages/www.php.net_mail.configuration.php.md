# Runtime Configuration




<div class="phpcode"><span class="html">
On Ubuntu 13.04, not sure of the other Distros. <br><br>If you simply uncomment the default:<br><br>sendmail_path = &quot;sendmail -t -i&quot;<br><br>Your mail() functions will all fail.&#xA0; This is because, you should place the FULL PATH (i.e.&#xA0; /usr/sbin/sendmail -t -i ) <br><br>The documentation states PHP tries it&apos;s best to find the correct sendmail path, but it clearly failed for me.<br><br>So, always enter in the FULLPATH to sendmail or you may get unexpected failing results.<br><br>As a secondary note:&#xA0; Those that just want to ENFORCE the -f parameter, you can do so in php.ini using: <br><br>mail.force_extra_parameters = -fdo_not_reply@domain.tld<br><br>You can leave the sendmail path commented out, it will still use the defaults&#xA0; (under UNIX&#xA0; -t -i options which if you look them up are very important to have set)....<br><br>But, now there is no way to change this, even with the 5th argument of the mail() function.&#xA0; -f is important, because if NOT set, will be set to which ever user the PHP script is running under, and you may not want that.<br><br>Also, -f&#xA0; sets the Return-Path:&#xA0; header which is used as the Bounce address, if errors occur, so you can process them.&#xA0; You you can not set Return-Path: in mail() headers for some reason... you could before.&#xA0; Now you have to use the -f option.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mail.configuration.php)

**[To root](/README.md)**