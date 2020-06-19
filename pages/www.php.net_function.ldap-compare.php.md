# ldap_compare




<div class="phpcode"><span class="html">
Just a side note that this is not how you&apos;d ever AUTHENTICATE someone, just an example code.<br><br>The common way to authenticate is to get the users name, use search and perhaps selection to the user to get her DN (single value) then attempt to BIND to the ldapserver using that dn and the offered password.&#xA0; If it works, then it&apos;s the right password.<br><br>Note that the password offered MUST NOT BE EMPTY or many LDAPs will presume you meant to authenticate anonymously and it will succeed, leaving you thinking it&apos;s the right password.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-compare.php)

**[To root](/README.md)**