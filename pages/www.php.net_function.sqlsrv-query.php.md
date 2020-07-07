# sqlsrv_query





If you are getting an error while attempting to execute your query, and the output of sqlsrv_errors(SQLSRV_ERR_ERRORS) is this:

SQLSTATE: IMSSP
code: -14
message: An invalid parameter was passed to sqlsrv_query.

You have failed to pass a valid parameter to sqlsrv_query itself, which could be one of three parameters:
Connection: a valid handled for a SQL Server Connection
Query: a valid string containing your query, with placeholders for parameters:&quot;(?)&quot; 
Parameters: An Array containing the values for your query parameters.&#xA0; (optional, but much match the number of placeholders in your Query.

I could not find any information about this error, and it turned out to be a missing connection parameter. In my case I found I had typed &quot;$connn&quot; instead of &quot;$conn&quot; in the code: 
if ($stmt=sqlsrv_query($conn, $sql, $params)) { ...

While this seems like a total &quot;noobie&quot; thing to do, the fact of the matter is there is very little information about this SQL Server Error message itself. So, the plain meaning of SQLSTATE &quot;IMSSP&quot;, CODE &quot;-14&quot; is that you provided no valid connection object to your sqlsrv_query function.

This message may appear baffling, especially if you have several occurrences of sqlsrv_query on a page, and you may have added a new occurrence after you closed your connection.

Since I wasted an enormous amount of time tracing the normal channels, I thought referencing this error here would provide some help. In was hung up on &quot;parameter&quot; and was thinking it was a bad parameter object, and overlooked passing an undefined connection object to sqlsrv_query

  

#

[Official documentation page](https://www.php.net/manual/en/function.sqlsrv-query.php)

**[To root](/README.md)**