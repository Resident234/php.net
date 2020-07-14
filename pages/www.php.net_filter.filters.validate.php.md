# Validate filters



FILTER_VALIDATE_URL does not work with URNs, examples of valid URIs according to RFC3986 and if they are accepted by FILTER_VALIDATE_URL:<br><br>[PASS] ftp://ftp.is.co.za.example.org/rfc/rfc1808.txt<br>[PASS] gopher://spinaltap.micro.umn.example.edu/00/Weather/California/Los%20Angeles<br>[PASS] http://www.math.uio.no.example.net/faq/compression-faq/part1.html<br>[PASS] mailto:mduerst@ifi.unizh.example.gov<br>[PASS] news:comp.infosystems.www.servers.unix<br>[PASS] telnet://melvyl.ucop.example.edu/<br>[PASS] http://www.ietf.org/rfc/rfc2396.txt<br>[PASS] ldap://[2001:db8::7]/c=GB?objectClass?one<br>[PASS] mailto:John.Doe@example.com<br>[PASS] news:comp.infosystems.www.servers.unix<br>[FAIL] tel:+1-816-555-1212<br>[PASS] telnet://192.0.2.16:80/<br>[FAIL] urn:oasis:names:specification:docbook:dtd:xml:4.1.2  

#

Notably missing is a way to validate text entry as printable,<br>printable multiline,<br>or printable and safe (tag free)<br><br>FILTER_VALIDATE_TEXT, which validates no special characters<br>perhaps with FILTER_FLAG_ALLOW_NEWLINE<br>and FILTER_FLAG_NOTAG to disallow tag starters  

#

FILTER_VALIDATE_EMAIL does NOT allow incomplete e-mail addresses to be validated as mentioned by Tomas.<br><br>Using the following code:<br><br>

```
<?php
$email = "clifton@example"; //Note the .com missing
echo "PHP Version: ".phpversion().&apos;&lt;br&gt;&apos;;
if(filter_var($email, FILTER_VALIDATE_EMAIL)){
    echo $email.&apos;&lt;br&gt;&apos;;
    var_dump(filter_var($email, FILTER_VALIDATE_EMAIL));
}else{
    var_dump(filter_var($email, FILTER_VALIDATE_EMAIL));    
}
?>
```


Returns:
PHP Version: 5.2.14 //On MY server, may be different depending on which version you have installed.
bool(false)

While the following code:



```
<?php
$email = "clifton@example.com"; //Note the .com added
echo "PHP Version: ".phpversion().&apos;&lt;br&gt;&apos;;
if(filter_var($email, FILTER_VALIDATE_EMAIL)){
    echo $email.&apos;&lt;br&gt;&apos;;
    var_dump(filter_var($email, FILTER_VALIDATE_EMAIL));
}else{
    var_dump(filter_var($email, FILTER_VALIDATE_EMAIL));    
}
?>
```
<br><br>Returns:<br>PHP Version: 5.2.14 //On MY server, may be different depending on which version you have installed.<br>clifton@example.com<br>string(16) "clifton@example.com"<br><br>This feature is only available for PHP Versions (PHP 5 &gt;= 5.2.0) according to documentation. So make sure your version is correct.<br><br>Cheers,<br>Clifton  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.validate.php)

**[To root](/README.md)**