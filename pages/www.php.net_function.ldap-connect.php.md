# ldap_connect




<div class="phpcode"><span class="html">
If you don&apos;t want your PHP program to wait XXX seconds before giving up in a case when one of your corporate DC have failed, and since ldap_connect() does not have a mechanism to timeout on a user specified time, this is my workaround which shows excellent practical results.<br><br>===========================================================<br>function serviceping($host, $port=389, $timeout=1)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; $op = fsockopen($host, $port, $errno, $errstr, $timeout);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!$op) return 0; //DC is N/A<br>&#xA0; &#xA0; else {<br>&#xA0; &#xA0; fclose($opanak); //explicitly close open socket connection<br>&#xA0; &#xA0; return 1; //DC is up &amp; running, we can safely connect with ldap_connect<br>&#xA0; &#xA0; }<br>}<br><br>// ##### STATIC DC LIST, if your DNS round robin is not setup<br>//$dclist = array(&apos;10.111.222.111&apos;, &apos;10.111.222.100&apos;, &apos;10.111.222.200&apos;);<br><br>// ##### DYNAMIC DC LIST, reverse DNS lookup sorted by round-robin result<br>$dclist = gethostbynamel(&apos;domain.name&apos;);<br><br>foreach ($dclist as $k =&gt; $dc) if (serviceping($dc) == true) break; else $dc = 0;<br>//after this loop, either there will be at least one DC which is available at present, or $dc would return bool false while the next line stops program from further execution<br><br>if (!$dc) exit(&quot;NO DOMAIN CONTROLLERS AVAILABLE AT PRESENT, PLEASE TRY AGAIN LATER!&quot;); //user being notified<br><br>//now, ldap_connect would certainly connect succesfully to DC tested previously and no timeout will occur<br>$ldapconn = ldap_connect($dc) or die(&quot;DC N/A, PLEASE TRY AGAIN LATER.&quot;);<br>===========================================================<br><br>Also with this approach, you get a real nice failover functionality, take for an example a company with a dozen of DC-a distributed along distant places, this way your PHP program will always have high availability if at least one DC is active at present.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To be able to make modifications to Active Directory via the LDAP connector you must bind to the LDAP service over SSL. Otherwise Active Directory provides a mostly readonly connection. You cannot add objects or modify certain properties without LDAPS, e.g. passwords can only be changed using LDAPS connections to Active Directory.<br><br>Therefore, for those wishing to securely connect to Active Directory, from a Unix host using PHP+OpenLDAP+OpenSSL I spent some time getting this going myself, and came across a few gotcha&apos;s. Hope this proves fruitfull for others like me when you couldn&apos;t find answers out there.<br><br>Make sure you compile OpenLDAP with OpenSSL support, and that you compile PHP with OpenLDAP and OpenSSL.<br><br>This provides PHP with what it needs to make use of ldaps:// connections.<br><br>Configure OpenSSL:<br><br>Extract your Root CA certificate from Active Directory, this is achived through the use of Certificate Services, a startard component of Windows 2000 Server, but may not be installed by default, (The usual Add/Remove Software method will work here). I extracted this in Base64 not DER format.<br><br>Place the extracted CAcert into the certs folder for openssl. (e.g. /usr/local/ssl/certs) and setup the hashed symlinks. This is easily done by simply running:<br><br>&#xA0; /usr/local/ssl/bin/c_rehash<br><br>Once this is done you can test it is worked by running:<br><br>&#xA0; /usr/local/ssl/bin/openssl verify -verbose -CApath /usr/local/ssl/certs /tmp/exported_cacert.pem<br><br>(Should return: OK).<br><br>Configure OpenLDAP:<br><br>Add the following to your ldap.conf file.<br>(found as /usr/local/openldap/etc/openldap/ldap.conf)<br><br>&#xA0; #--begin--<br><br>&#xA0; # Instruct client to NOT request a server&apos;s cert.<br>&#xA0; TLS_REQCERT never<br><br>&#xA0; # Define location of CA Cert<br>&#xA0; TLS_CACERT /usr/local/ssl/certs/AD_CA_CERT.pem<br>&#xA0; TLS_CACERTDIR /usr/local/ssl/certs<br><br>&#xA0; #--end--<br><br>You also need to place those same settings in a file within the Apache Web user homedir called .ldaprc<br><br>&#xA0; e.g.:<br>&#xA0; <br>&#xA0; cp /usr/local/openldap/etc/openldap/ldap.conf ~www/.ldaprc )<br><br>You can then test that you&apos;re able to establish a LDAPS connection to Active Directory from the OpenLDAP command tools:<br><br>&#xA0; /usr/local/openldap/bin/ldapsearch -H &quot;ldaps://adserver.ad.com&quot;<br><br>This should return some output in extended LDIF format and will indicate no matching objects, but it proves the connection works.<br><br>The name of the server you&apos;re connecting to is important. If they server name you specify in the &quot;ldaps://&quot; URI does not match the name of the server in it&apos;s certificate, it will complain like so:<br><br>&#xA0; ldap_bind: Can&apos;t contact LDAP server (81)<br>&#xA0; &#xA0; &#xA0; &#xA0; additional info: TLS: hostname does not match CN in peer certificate<br><br>Once you&apos;ve gotten the ldapsearch tool working correctly PHP should work also.<br><br>One important gotcha however is that the Web user must be able to locate it&apos;s HOME folder. You must check that Apache is providing a HOME variable set to the Web users home directory, so that php can locate the .ldaprc file and the settings contained within. This may well be different between Unix variants but it is such a simple and stupid thing if you miss it and it causes you grief. Simply use a SetEnv directive in Apache&apos;s httpd.conf:<br><br>&#xA0; SetEnv HOME /usr/local/www<br><br>With all that done, you can now code up a simple connect function:<br><br>&#xA0; function connect_AD()<br>&#xA0; {<br>&#xA0; &#xA0; $ldap_server = &quot;ldaps://adserver.ad.com&quot; ;<br>&#xA0; &#xA0; $ldap_user&#xA0;&#xA0; = &quot;CN=web service account,OU=Service Accounts,DC=ad,DC=com&quot; ;<br>&#xA0; &#xA0; $ldap_pass&#xA0;&#xA0; = &quot;password&quot; ;<br><br>&#xA0; &#xA0; $ad = ldap_connect($ldap_server) ;<br>&#xA0; &#xA0; ldap_set_option($ad, LDAP_OPT_PROTOCOL_VERSION, 3) ;<br>&#xA0; &#xA0; $bound = ldap_bind($ad, $ldap_user, $ldap_pass);<br><br>&#xA0; &#xA0; return $ad ;<br>&#xA0; }<br><br>Optionally you can avoid the URI style server string and use something like ldap_connect(&quot;adserver.ad.com&quot;, 636) ;&#xA0; But work fine with Active Directory servers.<br><br>Hope this proves usefull.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-connect.php)

**[To root](/README.md)**