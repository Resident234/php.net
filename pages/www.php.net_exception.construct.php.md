# Exception::__construct





For those that haven&apos;t done exception chaining. Here&apos;s an example. 

This allows you to add the previous exception to the next one and give yourself detailed information in the end as to what happened. This is useful in larger applications.



```
<?php
function theDatabaseObj(){
&#xA0; &#xA0;&#xA0; if( database_object ){
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return database_object; 
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; else{
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new DatabaseException(&quot;Could not connect to the database&quot;);
&#xA0; &#xA0;&#xA0; }
}

function updateProfile( $userInfo ){
&#xA0; &#xA0;&#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $db = theDatabaseObj();
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $db-&gt;updateProfile();
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; catch( DatabaseException $e ){
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $message = &quot;The user :&quot; . $userInfo-&gt;username . &quot; could not update his profile information&quot;;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; /* notice the &apos;$e&apos;. I&apos;m adding the previous exception&#xA0; to this exception. I can later get a detailed view of 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; where the problem began. Lastly, the number &apos;12&apos; is&#xA0; an exception code. I can use this for categorizing my 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; exceptions or don&apos;t use it at all. */ 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new MemberSettingsException($message,12,$e);
&#xA0; &#xA0;&#xA0; }
}

try{
&#xA0; &#xA0;&#xA0; updateProfile( $userInfo );
}
catch( MemberSettingsException $e ){
&#xA0; &#xA0;&#xA0; // this will give all information we have collected above. 
&#xA0; &#xA0;&#xA0; echo $e-&gt;getTraceAsString();
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/exception.construct.php)

**[To root](/README.md)**