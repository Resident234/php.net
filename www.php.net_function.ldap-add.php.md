# ldap_add




<div class="phpcode"><span class="html">
Try this script if you don&apos;t know how to add an user in the AD Win2K.<br>To have more informations about the attributes, open the adsiedit console in the Support Tools for Win2K.<br><br>$adduserAD[&quot;cn&quot;][0] = <br>$adduserAD[&quot;instancetype&quot;][0] = <br>$adduserAD[&quot;samaccountname&quot;][0] = <br>$adduserAD[&quot;objectclass&quot;][0] = &quot;top&quot;;<br>$adduserAD[&quot;objectclass&quot;][1] = &quot;person&quot;;<br>$adduserAD[&quot;objectclass&quot;][2] = &quot;organizationalPerson&quot;;<br>$adduserAD[&quot;objectclass&quot;][3] = &quot;user&quot;;<br>$adduserAD[&quot;displayname&quot;][0] = <br>$adduserAD[&quot;name&quot;][0] = <br>$adduserAD[&quot;givenname&quot;][0] = <br>$adduserAD[&quot;sn&quot;][0] = <br>$adduserAD[&quot;company&quot;][0] =<br>$adduserAD[&quot;department&quot;][0] = <br>$adduserAD[&quot;title&quot;][0] = <br>$adduserAD[&quot;description&quot;][0] = <br>$adduserAD[&quot;mail&quot;][0] = <br>$adduserAD[&quot;initials&quot;][0] = <br>$adduserAD[&quot;samaccountname&quot;][0] = <br>$adduserAD[&quot;userprincipalname&quot;][0] = <br>$adduserAD[&quot;profilepath&quot;][0] =<br>$adduserAD[&quot;manager&quot;][0] = ***Use DistinguishedName***<br><br>if (!($ldap = ldap_connect(&quot;localhost&quot;))) { <br>&#xA0; &#xA0;&#xA0; die (&quot;Could not connect to LDAP server&quot;); <br>}<br>if (!($res = @ldap_bind($ldap, &quot;user@pc.com&quot;, $password))) { <br>&#xA0; &#xA0;&#xA0; die (&quot;Could not bind to the LDAP account&quot;); <br>}<br>if (!(ldap_add($ldap, &quot;CN=New User,OU=OU Users,DC=pc,DC=com&quot;, $adduserAD))){<br>&#xA0; &#xA0;&#xA0; echo &quot;There is a problem to create the account<br>&#xA0; &#xA0;&#xA0; echo &quot;Please contact your administrator !&quot;;<br>&#xA0; &#xA0;&#xA0; exit;<br>}<br>ldap_unbind($ldap);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-add.php)

**[⬆ to root](/)**