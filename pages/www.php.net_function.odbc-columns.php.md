# odbc_columns





[MS SQL Server 2005/2008, PHP 5]

Imagine you would need to access the column names of a specific table, for instance to display them as table headers for fields with missing information. While browsing the documentation I was kind of lost how to use odbc_columns() without the usage of odbc_result_all() which outputs EVERYTHING in a single HTML table.

Here is a way to stuff all output into an array and then access only one or more fields of the odbc_columns() output:



```
<?php
include(&apos;connect.inc&apos;); // &lt;== Put all your database connection parameters in here. (DSN, PWD, USR, mssql_connect, etc.; returns $connection)

$outval = odbc_columns($connection, &quot;your DB name&quot;, &quot;%&quot;, &quot;your table name&quot;, &quot;%&quot;);

$pages = array();
while (odbc_fetch_into($outval, $pages)) {
&#xA0; &#xA0; &#xA0; &#xA0; echo $pages[3] . &quot;&lt;br /&gt;\n&quot;; // presents all fields of the array $pages in a new line until the array pointer reaches the end of array data
&#xA0; &#xA0; }
?>
```


Now your array $pages will have the following contents:
([x] is the array index displayed here for better understanding)

[0] TABLE_CAT &lt;== your DB name
[1] TABLE_SCHEM &lt;== dbo, your table scheme
[2] TABLE_NAME &lt;== your table name
[3] COLUMN_NAME &lt;== your column names (selected all with &quot;%&quot; in odbc_columns() )
[4] DATA_TYPE &lt;== -8
[5] TYPE_NAME &lt;== nchar (corresponds to -8, 11 f.i. is datetime and so on)
[6] COLUMN_SIZE &lt;== num. val.
[7] BUFFER_LENGTH &lt;== num. val.
[8] DECIMAL_DIGITS &lt;== num. val. or NULL
[9] NUM_PREC_RADIX &lt;== num. val. or NULL
[10] NULLABLE &lt;== num. val.
[11] REMARKS &lt;== num. val. or NULL
[12] COLUMN_DEF &lt;== num. val. or NULL
[13] SQL_DATA_TYPE &lt;== num. val.
[14] SQL_DATETIME_SUB &lt;== num. val. or NULL
[15] CHAR_OCTET_LENGTH &lt;== num. val. or NULL
[16] ORDINAL_POSITION &lt;== num. val.
[17] IS_NULLABLE &lt;== YES/NO
[18] SS_DATA_TYPE &lt;== num. val.

Now you can access each field recursivly by its key and output only the DESIRED fields instead of having ALL output from odbc_result_all().
Please note that the array key starts at zero (0) instead of one (1), so echo $pages[3] selects COLUMN_NAME from the above list.

I hope this helps...

Cheers
Thomas

  

#

[Official documentation page](https://www.php.net/manual/en/function.odbc-columns.php)

**[To root](/README.md)**