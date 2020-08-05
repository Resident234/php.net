# ldap_add



Try this script if you don&apos;t know how to add an user in the AD Win2K.<br>To have more informations about the attributes, open the adsiedit console in the Support Tools for Win2K.<br><br>$adduserAD["cn"][0] = <br>$adduserAD["instancetype"][0] = <br>$adduserAD["samaccountname"][0] = <br>$adduserAD["objectclass"][0] = "top";<br>$adduserAD["objectclass"][1] = "person";<br>$adduserAD["objectclass"][2] = "organizationalPerson";<br>$adduserAD["objectclass"][3] = "user";<br>$adduserAD["displayname"][0] = <br>$adduserAD["name"][0] = <br>$adduserAD["givenname"][0] = <br>$adduserAD["sn"][0] = <br>$adduserAD["company"][0] =<br>$adduserAD["department"][0] = <br>$adduserAD["title"][0] = <br>$adduserAD["description"][0] = <br>$adduserAD["mail"][0] = <br>$adduserAD["initials"][0] = <br>$adduserAD["samaccountname"][0] = <br>$adduserAD["userprincipalname"][0] = <br>$adduserAD["profilepath"][0] =<br>$adduserAD["manager"][0] = ***Use DistinguishedName***<br><br>if (!($ldap = ldap_connect("localhost"))) { <br>     die ("Could not connect to LDAP server"); <br>}<br>if (!($res = @ldap_bind($ldap, "user@pc.com", $password))) { <br>     die ("Could not bind to the LDAP account"); <br>}<br>if (!(ldap_add($ldap, "CN=New User,OU=OU Users,DC=pc,DC=com", $adduserAD))){<br>     echo "There is a problem to create the account<br>     echo "Please contact your administrator !";<br>     exit;<br>}<br>ldap_unbind($ldap);  

---

[Official documentation page](https://www.php.net/manual/en/function.ldap-add.php)

**[To root](/README.md)**