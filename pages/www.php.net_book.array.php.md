# Arrays



For newbies like me.<br><br>Creating new arrays:-<br>//Creates a blank array.<br>$theVariable = array();<br><br>//Creates an array with elements.<br>$theVariable = array("A", "B", "C");<br><br>//Creating Associaive array.<br>$theVariable = array(1 =&gt; "http//google.com", 2=&gt; "http://yahoo.com");<br><br>//Creating Associaive array with named keys<br>$theVariable = array("google" =&gt; "http//google.com", "yahoo"=&gt; "http://yahoo.com");<br><br>Note:<br>New value can be added to the array as shown below.<br>$theVariable[] = "D";<br>$theVariable[] = "E";  

#

To delete an individual array element use the unset function<br><br>For example:<br><br>&lt;?PHP<br>    $arr = array( "A", "B", "C" );<br>    unset( $arr[1] );<br>    // now $arr = array( "A", "C" );<br>?>
```
<br><br>Unlink is for deleting files.  

#

[Official documentation page](https://www.php.net/manual/en/book.array.php)

**[To root](/README.md)**