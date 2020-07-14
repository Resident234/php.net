# mysqli::commit



I never recomend to use the ? with only one value variant like: $var = expression ? $var  : other_value or $var = expression ? null  : other_value ,and php suport Exception catchin so,use it :)<br><br>here my opinion abut lorenzo&apos;s post:<br><br>  

```
<?php
 
//variants combined

$mysqli-&gt;autocommit(FALSE);

try{

  $mysqli-&gt;query("INSERT INTO myCity (id) VALUES (100)") or throw new Exception(&apos;error!&apos;);

// or we can use

  if( !$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (200)"){ 
    throw new Exception(&apos;error!&apos;); 
  }

}catch( Exception $e ){
  $mysqli-&gt;rollback();
}
$mysqli-&gt;commit();

?>
```
  

#

Please note that calling mysqli::commit() will NOT automatically set mysqli::autocommit() back to &apos;true&apos;.<br><br>This means that any queries following mysqli::commit() will be rolled back when your script exits.  

#

This is an example to explain the powerful of the rollback and commit functions.<br>Let&apos;s suppose you want to be sure that all queries have to be executed without errors before writing data on the database.<br>Here&apos;s the code:<br><br>

```
<?php
$all_query_ok=true; // our control variable

//we make 4 inserts, the last one generates an error
//if at least one query returns an error we change our control variable
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (100)") ? null : $all_query_ok=false;
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (200)") ? null : $all_query_ok=false;
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (300)") ? null : $all_query_ok=false;
$mysqli-&gt;query("INSERT INTO myCity (id) VALUES (100)") ? null : $all_query_ok=false; //duplicated PRIMARY KEY VALUE

//now let&apos;s test our control variable
$all_query_ok ? $mysqli-&gt;commit() : $mysqli-&gt;rollback();

$mysqli-&gt;close();
?>
```
<br><br>hope to be helpful!  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.commit.php)

**[To root](/README.md)**