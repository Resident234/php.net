# mysqli::commit





I never recomend to use the ? with only one value variant like: $var = expression ? $var&#xA0; : other_value or $var = expression ? null&#xA0; : other_value ,and php suport Exception catchin so,use it :)

here my opinion abut lorenzo&apos;s post:

&#xA0; 

```
<?php
 
//variants combined

$mysqli-&gt;autocommit(FALSE);

try{

&#xA0; $mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (100)&quot;) or throw new Exception(&apos;error!&apos;);

// or we can use

&#xA0; if( !$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (200)&quot;){ 
&#xA0; &#xA0; throw new Exception(&apos;error!&apos;); 
&#xA0; }

}catch( Exception $e ){
&#xA0; $mysqli-&gt;rollback();
}
$mysqli-&gt;commit();

?>
```



  

#



Please note that calling mysqli::commit() will NOT automatically set mysqli::autocommit() back to &apos;true&apos;.

This means that any queries following mysqli::commit() will be rolled back when your script exits.

  

#



This is an example to explain the powerful of the rollback and commit functions.
Let&apos;s suppose you want to be sure that all queries have to be executed without errors before writing data on the database.
Here&apos;s the code:



```
<?php
$all_query_ok=true; // our control variable

//we make 4 inserts, the last one generates an error
//if at least one query returns an error we change our control variable
$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (100)&quot;) ? null : $all_query_ok=false;
$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (200)&quot;) ? null : $all_query_ok=false;
$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (300)&quot;) ? null : $all_query_ok=false;
$mysqli-&gt;query(&quot;INSERT INTO myCity (id) VALUES (100)&quot;) ? null : $all_query_ok=false; //duplicated PRIMARY KEY VALUE

//now let&apos;s test our control variable
$all_query_ok ? $mysqli-&gt;commit() : $mysqli-&gt;rollback();

$mysqli-&gt;close();
?>
```


hope to be helpful!

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.commit.php)

**[To root](/README.md)**