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
        echo "LDAP bind successful...&lt;br /&gt;&lt;br /&gt;";
        
        
        $result = ldap_search($ldapconn,$ldaptree, "(cn=*)") or die ("Error in search query: ".ldap_error($ldapconn));
        $data = ldap_get_entries($ldapconn, $result);
        
        // SHOW ALL DATA
        echo '&lt;h1&gt;Dump all data&lt;/h1&gt;&lt;pre&gt;';
        print_r($data);    
        echo '&lt;/pre&gt;';
        
        
        // iterate over array and print data for each entry
        echo '&lt;h1&gt;Show me the users&lt;/h1&gt;';
        for ($i=0; $i&lt;$data["count"]; $i++) {
            //echo "dn is: ". $data[$i]["dn"] ."&lt;br /&gt;";
            echo "User: ". $data[$i]["cn"][0] ."&lt;br /&gt;";
            if(isset($data[$i]["mail"][0])) {
                echo "Email: ". $data[$i]["mail"][0] ."&lt;br /&gt;&lt;br /&gt;";
            } else {
                echo "Email: None&lt;br /&gt;&lt;br /&gt;";
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