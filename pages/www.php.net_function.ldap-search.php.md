# ldap_search





```
<?php
set_time_limit(30);
error_reporting(E_ALL);
ini_set('error_reporting', E_ALL);
ini_set('display_errors',1);

// config
$ldapserver = 'svr.domain.com';
$ldapuser      = 'administrator';  
$ldappass     = 'PASSWORD_HERE';
$ldaptree    = "OU=SBSUsers,OU=Users,OU=MyBusiness,DC=myDomain,DC=local";

// connect 
$ldapconn = ldap_connect($ldapserver) or die("Could not connect to LDAP server.");

if($ldapconn) {
    // binding to ldap server
    $ldapbind = ldap_bind($ldapconn, $ldapuser, $ldappass) or die ("Error trying to bind: ".ldap_error($ldapconn));
    // verify binding
    if ($ldapbind) {
        echo "LDAP bind successful...<br /><br />";
        
        
        $result = ldap_search($ldapconn,$ldaptree, "(cn=*)") or die ("Error in search query: ".ldap_error($ldapconn));
        $data = ldap_get_entries($ldapconn, $result);
        
        // SHOW ALL DATA
        echo '<h1>Dump all data</h1><pre>';
        print_r($data);    
        echo '</pre>';
        
        
        // iterate over array and print data for each entry
        echo '<h1>Show me the users</h1>';
        for ($i=0; $i<$data["count"]; $i++) {
            //echo "dn is: ". $data[$i]["dn"] ."<br />";
            echo "User: ". $data[$i]["cn"][0] ."<br />";
            if(isset($data[$i]["mail"][0])) {
                echo "Email: ". $data[$i]["mail"][0] ."<br /><br />";
            } else {
                echo "Email: None<br /><br />";
            }
        }
        // print number of entries found
        echo "Number of entries found: " . ldap_count_entries($ldapconn, $result);
    } else {
        echo "LDAP bind failed...";
    }

}

// all done? clean up
ldap_close($ldapconn);
?>
```
  

#

Here are a couple of resources for proper construction of filters. <br><br>http://msdn2.microsoft.com/En-US/library/aa746475.aspx<br><br>http://technet.microsoft.com/en-us/library/aa996205.aspx<br><br>Before finding these I had been stumped for hours on how to do something like "all users starting with "a" except those from OU &apos;foo&apos;"  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-search.php)

**[To root](/README.md)**