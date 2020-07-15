# SQLite3Result::fetchArray



Would just like to point out for clarification that each call to fetchArray() returns the next result from SQLite3Result in an array, until there are no more results, whereupon the next fetchArray() call will return false.<br><br>HOWEVER an additional call of fetchArray() at this point will reset back to the beginning of the result set and once again return the first result. This does not seem to explicitly documented, and caused me my own fair share of headaches for a while until I figured it out.<br><br>For example:<br><br>

```
<?php 
        $returned_set = $database->query("select query or whatever");
        
        //Lets say the query returned 3 results
        //Normally the following while loop would run 3 times then, as $result wouldn't be false until the fourth call to fetchArray()
        while($result = $returned_set->fetchArray()) {
                //HOWEVER HAVING AN ADDITIONAL CALL IN THE LOOP WILL CAUSE THE LOOP TO RUN AGAIN
                $returned_set->fetchArray();
        }
?>
```
<br><br>Basically, in the above code fetchArray will return:<br>1st call | 1st result from $returned_set (fetchArray() call from while condition)<br>2nd call | 2nd result  (fetchArray() call from while block)<br>3rd call | 3rd result  (fetchArray() call from while condition)<br>4th call |FALSE  (fetchArray() call from while block)<br>5th call | 1st result  (fetchArray() call from while condition)<br>....<br><br>This will cause (at least in this case) the while loop to run infinitely.  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3result.fetcharray.php)

**[To root](/README.md)**