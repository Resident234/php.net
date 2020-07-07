# SQLite3Result::fetchArray





Would just like to point out for clarification that each call to fetchArray() returns the next result from SQLite3Result in an array, until there are no more results, whereupon the next fetchArray() call will return false.

HOWEVER an additional call of fetchArray() at this point will reset back to the beginning of the result set and once again return the first result. This does not seem to explicitly documented, and caused me my own fair share of headaches for a while until I figured it out.

For example:



```
<?php 
&#xA0; &#xA0; &#xA0; &#xA0; $returned_set = $database-&gt;query(&quot;select query or whatever&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; //Lets say the query returned 3 results
&#xA0; &#xA0; &#xA0; &#xA0; //Normally the following while loop would run 3 times then, as $result wouldn&apos;t be false until the fourth call to fetchArray()
&#xA0; &#xA0; &#xA0; &#xA0; while($result = $returned_set-&gt;fetchArray()) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //HOWEVER HAVING AN ADDITIONAL CALL IN THE LOOP WILL CAUSE THE LOOP TO RUN AGAIN
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $returned_set-&gt;fetchArray();
&#xA0; &#xA0; &#xA0; &#xA0; }
?>
```


Basically, in the above code fetchArray will return:
1st call | 1st result from $returned_set (fetchArray() call from while condition)
2nd call | 2nd result&#xA0; (fetchArray() call from while block)
3rd call | 3rd result&#xA0; (fetchArray() call from while condition)
4th call |FALSE&#xA0; (fetchArray() call from while block)
5th call | 1st result&#xA0; (fetchArray() call from while condition)
....

This will cause (at least in this case) the while loop to run infinitely.

  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3result.fetcharray.php)

**[To root](/README.md)**