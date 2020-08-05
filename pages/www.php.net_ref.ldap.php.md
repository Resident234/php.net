# LDAP Functions



First of all, sorry for my English.<br>Here are two functions to check group membership and some others which can be useful for work with LDAP (Active Directory in this example).<br><br>index.php<br>---------<br><br>

```
<?php

$user = 'bob';
$password = 'zhlob';
$host = 'myldap';
$domain = 'mydomain.ex';
$basedn = 'dc=mydomain,dc=ex';
$group = 'SomeGroup';

$ad = ldap_connect("ldap://{$host}.{$domain}") or die('Could not connect to LDAP server.');
ldap_set_option($ad, LDAP_OPT_PROTOCOL_VERSION, 3);
ldap_set_option($ad, LDAP_OPT_REFERRALS, 0);
@ldap_bind($ad, "{$user}@{$domain}", $password) or die('Could not bind to AD.');
$userdn = getDN($ad, $user, $basedn);
if (checkGroupEx($ad, $userdn, getDN($ad, $group, $basedn))) {
//if (checkGroup($ad, $userdn, getDN($ad, $group, $basedn))) {
    echo "You're authorized as ".getCN($userdn);
} else {
    echo 'Authorization failed';
}
ldap_unbind($ad);

/*
* This function searchs in LDAP tree ($ad -LDAP link identifier)
* entry specified by samaccountname and returns its DN or epmty
* string on failure.
*/
function getDN($ad, $samaccountname, $basedn) {
    $attributes = array('dn');
    $result = ldap_search($ad, $basedn,
        "(samaccountname={$samaccountname})", $attributes);
    if ($result === FALSE) { return ''; }
    $entries = ldap_get_entries($ad, $result);
    if ($entries['count']>0) { return $entries[0]['dn']; }
    else { return ''; };
}

/*
* This function retrieves and returns CN from given DN
*/
function getCN($dn) {
    preg_match('/[^,]*/', $dn, $matchs, PREG_OFFSET_CAPTURE, 3);
    return $matchs[0][0];
}

/*
* This function checks group membership of the user, searching only
* in specified group (not recursively).
*/
function checkGroup($ad, $userdn, $groupdn) {
    $attributes = array('members');
    $result = ldap_read($ad, $userdn, "(memberof={$groupdn})", $attributes);
    if ($result === FALSE) { return FALSE; };
    $entries = ldap_get_entries($ad, $result);
    return ($entries['count'] > 0);
}

/*
* This function checks group membership of the user, searching
* in specified group and groups which is its members (recursively).
*/
function checkGroupEx($ad, $userdn, $groupdn) {
    $attributes = array('memberof');
    $result = ldap_read($ad, $userdn, '(objectclass=*)', $attributes);
    if ($result === FALSE) { return FALSE; };
    $entries = ldap_get_entries($ad, $result);
    if ($entries['count'] <= 0) { return FALSE; };
    if (empty($entries[0]['memberof'])) { return FALSE; } else {
        for ($i = 0; $i < $entries[0]['memberof']['count']; $i++) {
            if ($entries[0]['memberof'][$i] == $groupdn) { return TRUE; }
            elseif (checkGroupEx($ad, $entries[0]['memberof'][$i], $groupdn)) { return TRUE; };
        };
    };
    return FALSE;
}

?>
```
  

---

There is a lot of confusion about accountExpires, pwdLastSet, lastLogon and badPasswordTime active directory fields.<br><br>All of them are using "Interval" date/time format with a value that represents the number of 100-nanosecond intervals since January 1, 1601 (UTC, and a value of 0 or 0x7FFFFFFFFFFFFFFF, 9223372036854775807, indicates that the account never expires): https://msdn.microsoft.com/en-us/library/ms675098(v=vs.85).aspx<br><br>So if you need to translate it from/to UNIX timestamp you can easily calculate the difference with:<br><br>

```
<?php
$datetime1 = new DateTime('1601-01-01');
$datetime2 = new DateTime('1970-01-01');
$interval = $datetime1->diff($datetime2);
echo ($interval->days * 24 * 60 * 60) . " seconds\n";
?>
```


The difference between both dates is 11644473600 seconds. Don't rely on floating point calculations nor other numbers that probably were calculated badly (including time zone or something similar).

Now you can convert from LDAP field:



```
<?php
$lastlogon = $info[$i]['lastlogon'][0];
// divide by 10.000.000 to get seconds from 100-nanosecond intervals
$winInterval = round($lastlogon / 10000000);
// substract seconds from 1601-01-01 -> 1970-01-01
$unixTimestamp = ($winInterval - 11644473600);
// show date/time in local time zone
echo date("Y-m-d H:i:s", $unixTimestamp) ."\n";
?>
```
<br><br>Hope it helps.  

---

[Official documentation page](https://www.php.net/manual/en/ref.ldap.php)

**[To root](/README.md)**