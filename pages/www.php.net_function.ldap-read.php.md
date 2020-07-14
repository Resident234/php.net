# ldap_read



Clarification of the ldap_read command syntax:  <br><br>If you just want to pull certain attributes from an object and you already know it&apos;s dn, the ldap_read command can do this as illustrated below.  It will be less overhead than ldap_search.<br><br>The string base_dn which is normally used to set the top context for a recursive ldap_search is used slightly differently with this command.  It is used to specify the actual object with the full dn.  (Hopefully this saves someone else a couple hours trying this command out.)<br><br>

```
<?php
$ds = ldap.myserver.com // your ldap server
  $dn = "cn=username,o=My Company, c=US"; //the object itself instead of the top search level as in ldap_search
  $filter="(objectclass=*)"; // this command requires some filter
  $justthese = array("ou", "sn", "givenname", "mail"); //the attributes to pull, which is much more efficient than pulling all attributes if you don&apos;t do this
      $sr=ldap_read($ds, $dn, $filter, $justthese);
          $entry = ldap_get_entries($ds, $sr);

echo $entry[0]["mail"][0] . "is the email address of the cn your requested";
echo $entry[0]["sn"][0] . "is the sn of the cn your requested";
ldap_close($ds);
?>
```
 <br><br>This prints out the specified users mail and surname for example.  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-read.php)

**[To root](/README.md)**