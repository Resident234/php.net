# ldap_bind



Interesting point,<br><br>if you can&apos;t bind to active directory with the error "49: Invalid Credentials", you can get the extended error output from the ldap_get_option function, using the option: LDAP_OPT_DIAGNOSTIC_MESSAGE. Unfortunately php hasn&apos;t defined this by default, but it&apos;s value is 0x0032.<br><br>This is useful if a user must change their password at first login (Data: 773), or if their account has expired on the network (Data: 532).<br><br>

```
<?php

define(LDAP_OPT_DIAGNOSTIC_MESSAGE, 0x0032)

$handle = ldap_connect('ldap://active.directory.server/');
$bind = ldap_bind($handle, 'user', 'expiredpass');

if ($bind) {
    if (ldap_get_option($handle, LDAP_OPT_DIAGNOSTIC_MESSAGE, $extended_error)) {
        echo "Error Binding to LDAP: $extended_error";
    } else {
        echo "Error Binding to LDAP: No additional information is available.";
    }
}
?>
```
<br><br>Or something to that effect..<br><br>It took me a while to work this one out, so i figured i&apos;d share my results..  

#

I couldn&apos;t get ldap_bind to work on an ldaps connection until I followed some instructions about creating an ldap.conf file.  I don&apos;t see these instructions anywhere on the php site.  Maybe they&apos;re on the OpenLDAP site, but I thought it would be useful to have here as well.  Credit goes to a dude known as &apos;LRM&apos;, and I found my solution here: http://lists.horde.org/archives/sork/Week-of-Mon-20040503/001578.html<br><br>My setup is XAMPP on Win XP.<br>###### ApacheFriends XAMPP (basic package) version 1.6.3a ######<br><br>  + Apache 2.2.4<br>  + MySQL 5.0.45<br>  + PHP 5.2.3 + PHP 4.4.7 + PEAR<br>  + PHP-Switch win32 1.0 (please use the "php-switch.bat")<br>  + XAMPP Control Version 2.5 from www.nat32.com    <br>  + XAMPP Security 1.0    <br>  + SQLite 2.8.15<br>  + OpenSSL 0.9.8e<br>  + phpMyAdmin 2.10.3<br>  + ADOdb 4.95<br>  + Mercury Mail Transport System v4.01b<br>  + FileZilla FTP Server 0.9.23<br>  + Webalizer 2.01-10<br>  + Zend Optimizer 3.3.0<br>  + eAccelerator 0.9.5.1 for PHP 5.2.3  (comment out in the php.ini)<br><br>1. create C:\OpenLDAP\sysconf\ldap.conf (Yes, it MUST be this path because it&apos;s hard-coded in the dll)<br>2. put this line at the top:<br><br>TLS_REQCERT never<br><br>3. Save, stop/start apache.<br><br>The reason is, I think, because it doesn&apos;t understand the certificate, so this directive tells it to not bother checking it.  I guess that could be unsafe in some cases, but in my case I&apos;m confident with the server I&apos;m connecting to.<br><br>My connection code was as follows (nothing new here, I don&apos;t think):<br><br>

```
<?php
$con = @ldap_connect('ldaps://the.ldap.server', 636);
ldap_set_option($con, LDAP_OPT_PROTOCOL_VERSION, 3);
ldap_set_option($con, LDAP_OPT_REFERRALS, 0);
var_dump(@ldap_bind($con, 'user@sub.domain.com', 'password'));
?>
```
<br><br>Good luck!  LDAPS can be a real bitch.  

#

A number of examples and implementations of authentication schemes which use LDAP simple binds to authenticate users fail to properly sanitize user-submitted data. This can allow for an anonymous user to authenticate to a web-based application as an existing user. Provided below is a brief description and example of how this vulnerability can arise. For more detailed information please visit the links at the bottom of this posting.<br><br>The bind operation of LDAP, as described in RFC 4513, provides a method which allows for authentication of users. For the Simple Authentication Method a user may use the anonymous authentication mechanism, the unauthenticated authentication mechanism, or the name/password authentication mechanism. The unauthenticated authentication mechanism is used when a client who desires to establish an anonymous authorization state passes a non-zero length distinguished name and a zero length password. Most LDAP servers either can be configured to allow this mechanism or allow it by default. Web-based applications which perform the simple bind operation with the client&apos;s credentials are at risk when an anonymous authorization state is established. This can occur when the web-based application passes a distinguished name and a zero length password to the LDAP server.<br>This is commonly encountered when no password is provided from the client to the web-based application. This situation is described in some of the postings found below. For this situation, the recommendations found in other postings is sufficient to prevent authentication bypass.<br>However, no prior postings at php.net describe a situation in which a client may pass a distinguished username and a password of non-zero length to the web-based application which results in an anonymous authorization state. Below is an example of this situation.<br><br>$dn="testuser";<br>$pass="\x00\x41";<br>if (empty($dn) or empty($pass)) { exit(); } //check for empty strings<br>//if (preg_match(&apos;/[^a-zA-Z]/&apos;,$dn) or preg_match(&apos;/[^a-zA-Z0-9\x20!@#$%^&amp;*()]/&apos;,$pass)) { exit(); } //check for expected values (whitelisting)<br>//if (preg_match(&apos;/\x00/&apos;,$dn) or preg_match(&apos;/\x00/&apos;,$pass)) { exit(); } //check for null byte (blacklisting)<br>$ldapconn=ldap_connect("192.0.2.2") or die("Could not connect to LDAP server.");<br>if ($ldapconn) {<br>        ldap_set_option($ldapconn, LDAP_OPT_PROTOCOL_VERSION, 3);<br>        $ldapbind=ldap_bind($ldapconn, $dn, $pass);<br>        if ($ldapbind) {<br>                echo("success");<br>        } else {<br>                echo("fail");<br>                }<br>        }<br><br>References:<br>http://security.okstate.edu  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-bind.php)

**[To root](/README.md)**