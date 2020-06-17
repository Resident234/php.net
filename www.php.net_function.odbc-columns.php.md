# odbc_columns




<div class="phpcode"><span class="html">
[MS SQL Server 2005/2008, PHP 5]<br><br>Imagine you would need to access the column names of a specific table, for instance to display them as table headers for fields with missing information. While browsing the documentation I was kind of lost how to use odbc_columns() without the usage of odbc_result_all() which outputs EVERYTHING in a single HTML table.<br><br>Here is a way to stuff all output into an array and then access only one or more fields of the odbc_columns() output:<br><br><span class="default">&lt;?php<br></span><span class="keyword">include(</span><span class="string">&apos;connect.inc&apos;</span><span class="keyword">); </span><span class="comment">// &lt;== Put all your database connection parameters in here. (DSN, PWD, USR, mssql_connect, etc.; returns $connection)<br><br></span><span class="default">$outval </span><span class="keyword">= </span><span class="default">odbc_columns</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">, </span><span class="string">&quot;your DB name&quot;</span><span class="keyword">, </span><span class="string">&quot;%&quot;</span><span class="keyword">, </span><span class="string">&quot;your table name&quot;</span><span class="keyword">, </span><span class="string">&quot;%&quot;</span><span class="keyword">);<br><br></span><span class="default">$pages </span><span class="keyword">= array();<br>while (</span><span class="default">odbc_fetch_into</span><span class="keyword">(</span><span class="default">$outval</span><span class="keyword">, </span><span class="default">$pages</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$pages</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] . </span><span class="string">&quot;&lt;br /&gt;\n&quot;</span><span class="keyword">; </span><span class="comment">// presents all fields of the array $pages in a new line until the array pointer reaches the end of array data<br>&#xA0; &#xA0; </span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>Now your array $pages will have the following contents:<br>([x] is the array index displayed here for better understanding)<br><br>[0] TABLE_CAT &lt;== your DB name<br>[1] TABLE_SCHEM &lt;== dbo, your table scheme<br>[2] TABLE_NAME &lt;== your table name<br>[3] COLUMN_NAME &lt;== your column names (selected all with &quot;%&quot; in odbc_columns() )<br>[4] DATA_TYPE &lt;== -8<br>[5] TYPE_NAME &lt;== nchar (corresponds to -8, 11 f.i. is datetime and so on)<br>[6] COLUMN_SIZE &lt;== num. val.<br>[7] BUFFER_LENGTH &lt;== num. val.<br>[8] DECIMAL_DIGITS &lt;== num. val. or NULL<br>[9] NUM_PREC_RADIX &lt;== num. val. or NULL<br>[10] NULLABLE &lt;== num. val.<br>[11] REMARKS &lt;== num. val. or NULL<br>[12] COLUMN_DEF &lt;== num. val. or NULL<br>[13] SQL_DATA_TYPE &lt;== num. val.<br>[14] SQL_DATETIME_SUB &lt;== num. val. or NULL<br>[15] CHAR_OCTET_LENGTH &lt;== num. val. or NULL<br>[16] ORDINAL_POSITION &lt;== num. val.<br>[17] IS_NULLABLE &lt;== YES/NO<br>[18] SS_DATA_TYPE &lt;== num. val.<br><br>Now you can access each field recursivly by its key and output only the DESIRED fields instead of having ALL output from odbc_result_all().<br>Please note that the array key starts at zero (0) instead of one (1), so echo $pages[3] selects COLUMN_NAME from the above list.<br><br>I hope this helps...<br><br>Cheers<br>Thomas</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.odbc-columns.php)

**[To root](/README.md)**