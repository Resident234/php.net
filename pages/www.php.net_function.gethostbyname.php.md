# gethostbyname



If you do a gethostbyname() and there is no trailing dot after a domainname that does not resolve, this domainname will ultimately be appended to the server-FQDN by nslookup.<br><br>So if you do a lookup for nonexistentdomainname.be your server may return the ip for nonexistentdomainname.be.yourhostname.com, which is the server-ip.<br><br>To avoid this behaviour, just add a trailing dot to the domainname; i.e. gethostbyname(&apos;nonexistentdomainname.be.&apos;)  

#

Options for the underlying resolver functions can be supplied by using the RES_OPTIONS environmental variable. (at least under Linux, see man resolv.conf)<br><br>Set timeout and retries to 1 to have a maximum execution time of 1 second for the DNS lookup:<br>

```
<?php
  putenv(&apos;RES_OPTIONS=retrans:1 retry:1 timeout:1 attempts:1&apos;);
  gethostbyname($something);
?>
```
<br><br>You should also use fully qualified domain names ending in a dot. This prevents the resolver from walking though all search domains and retrying the domain with the search domain appended.  

#

This function says "Returns the IPv4 address or a string containing the unmodified hostname on failure.<br><br>This isn&apos;t entirely true, any hostname with a null byte in it will only return the characters BEFORE the null byte.<br><br>

```
<?php
$hostname = "foo\0bar";
var_dump($hostname );
var_dump(gethostbyname($hostname ));
?>
```
<br><br>Results:<br>string &apos;foo&#xFFFD;bar&apos; (length=7)<br>string &apos;foo&apos; (length=3)  

#

Important note: You should avoid its use in production.<br><br>DNS Resolution may take from 0.5 to 4 seconds, and during this time your script is NOT being executed.<br><br>Your customers may think that the server is slow, but actually it is just waiting for the DNS resolution response.<br><br>You can use it, but if you want performance, you should avoid it, or schedule it to some CRON script...  

#

[Official documentation page](https://www.php.net/manual/en/function.gethostbyname.php)

**[To root](/README.md)**