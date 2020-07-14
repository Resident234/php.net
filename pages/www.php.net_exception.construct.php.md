# Exception::__construct



For those that haven&apos;t done exception chaining. Here&apos;s an example. <br><br>This allows you to add the previous exception to the next one and give yourself detailed information in the end as to what happened. This is useful in larger applications.<br><br>

```
<?php
function theDatabaseObj(){
     if( database_object ){
         return database_object; 
     }
     else{
         throw new DatabaseException("Could not connect to the database");
     }
}

function updateProfile( $userInfo ){
     try{
         $db = theDatabaseObj();
         $db-&gt;updateProfile();
     }
     catch( DatabaseException $e ){
         $message = "The user :" . $userInfo-&gt;username . " could not update his profile information";
         /* notice the &apos;$e&apos;. I&apos;m adding the previous exception  to this exception. I can later get a detailed view of 
          where the problem began. Lastly, the number &apos;12&apos; is  an exception code. I can use this for categorizing my 
         exceptions or don&apos;t use it at all. */ 
         throw new MemberSettingsException($message,12,$e);
     }
}

try{
     updateProfile( $userInfo );
}
catch( MemberSettingsException $e ){
     // this will give all information we have collected above. 
     echo $e-&gt;getTraceAsString();
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/exception.construct.php)

**[To root](/README.md)**