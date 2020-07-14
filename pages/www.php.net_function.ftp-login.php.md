# ftp_login



To suppress the PHP warning, just prepend the function with the error suppression character @. I&apos;m usually against error suppression, but apparently some genius thought it was a good idea to really drive the point home that you have a bad login. Returning false wasn&apos;t enough?<br><br>if( ! @ftp_login( $connection, &apos;USERNAME&apos;, &apos;PASSWORD&apos; ) ){<br>        die( &apos;Bad login, but no PHP warning thrown.&apos;);<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-login.php)

**[To root](/README.md)**