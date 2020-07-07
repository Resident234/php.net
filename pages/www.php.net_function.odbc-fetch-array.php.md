# odbc_fetch_array





I really liked Ryan&apos;s example so I took it another step and added a recordset class to work with the connection class.&#xA0; I made slight alterations to the original code as well.&#xA0; Also note the recordset class takes advantage of php5&apos;s __get property function...

&lt;%
class odbcRecordset {
&#xA0;&#xA0; var $recordcount;
&#xA0;&#xA0; var $currentrow;
&#xA0;&#xA0; var $eof;

&#xA0;&#xA0; var $recorddata;
&#xA0;&#xA0; var $query;

&#xA0;&#xA0; function odbcConnection(){
&#xA0; &#xA0; &#xA0; $this-&gt;recordcount = 0;
&#xA0; &#xA0; &#xA0; $this-&gt;recorddata = 0;
&#xA0;&#xA0; }

&#xA0;&#xA0; function SetData( $newdata, $num_records, $query ) {
&#xA0; &#xA0; &#xA0; $this-&gt;recorddata = $newdata;
&#xA0; &#xA0; &#xA0; $this-&gt;recordcount = $num_records;
&#xA0; &#xA0; &#xA0; $this-&gt;query = $query;
&#xA0; &#xA0; &#xA0; $this-&gt;currentrow = 0;
&#xA0; &#xA0; &#xA0; $this-&gt;set_eof();
&#xA0;&#xA0; }

&#xA0;&#xA0; function set_eof() {
&#xA0; &#xA0; &#xA0; $this-&gt;eof = $this-&gt;currentrow &gt;= $this-&gt;recordcount;
&#xA0;&#xA0; }

&#xA0;&#xA0; function movenext()&#xA0; { if ($this-&gt;currentrow &lt; $this-&gt;recordcount) { $this-&gt;currentrow++; $this-&gt;set_eof(); } }
&#xA0;&#xA0; function moveprev()&#xA0; { if ($this-&gt;currentrow &gt; 0)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; { $this-&gt;currentrow--; $this-&gt;set_eof(); } }
&#xA0;&#xA0; function movefirst() { $this-&gt;currentrow = 0; set_eof();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0;&#xA0; function movelast()&#xA0; { $this-&gt;currentrow = $this-&gt;recordcount - 1;&#xA0; set_eof();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }

&#xA0;&#xA0; function data($field_name) {
&#xA0; &#xA0; &#xA0; if (isset($this-&gt;recorddata[$this-&gt;currentrow][$field_name])) {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $thisVal = $this-&gt;recorddata[$this-&gt;currentrow][$field_name];
&#xA0; &#xA0; &#xA0; } else if ($this-&gt;eof) {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; die(&quot;&lt;B&gt;Error!&lt;/B&gt; eof of recordset was reached&quot;);
&#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; die(&quot;&lt;B&gt;Error!&lt;/B&gt; Field &lt;B&gt;&quot; . $field_name . &quot;&lt;/B&gt; was not found in the current recordset from query:&lt;br&gt;&lt;br&gt;$this-&gt;query&quot;);
&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; return $thisVal;
&#xA0;&#xA0; } 

&#xA0;&#xA0; function __get($field_name) {
&#xA0; &#xA0; &#xA0; return $this-&gt;data($field_name);
&#xA0;&#xA0; } 
}

class odbcConnection {
&#xA0;&#xA0; var $user;&#xA0; //Username for the database
&#xA0;&#xA0; var $pass; //Password

&#xA0;&#xA0; var $conn_handle; //Connection handle
&#xA0;&#xA0; var $temp_fieldnames; //Tempory array used to store the fieldnames, makes parsing returned data easier.
&#xA0;&#xA0; 
&#xA0;&#xA0; function odbcConnection(){
&#xA0; &#xA0; &#xA0; $this-&gt;user = &quot;&quot;;
&#xA0; &#xA0; &#xA0; $this-&gt;pass = &quot;&quot;;
&#xA0;&#xA0; }
&#xA0;&#xA0; 
&#xA0;&#xA0; function open($dsn,$user,$pass){
&#xA0; &#xA0; &#xA0; $handle = @odbc_connect($dsn,$user,$pass,SQL_CUR_USE_ODBC) or
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; die(&quot;&lt;B&gt;Error!&lt;/B&gt; Couldn&apos;t Connect To Database. Error Code:&#xA0; &quot;.odbc_error());
&#xA0; &#xA0; &#xA0; $this-&gt;conn_handle = $handle;
&#xA0; &#xA0; &#xA0; return true;
&#xA0;&#xA0; }
&#xA0;&#xA0; 
&#xA0;&#xA0; function &amp;execute($query){
&#xA0; &#xA0; &#xA0; //Create a temp recordset
&#xA0; &#xA0; &#xA0; $newRS = new odbcRecordset;
&#xA0; &#xA0; &#xA0; $thisData = &quot;&quot;;

&#xA0; &#xA0; &#xA0; $res = @odbc_exec($this-&gt;conn_handle,$query) or
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; die(&quot;&lt;B&gt;Error!&lt;/B&gt; Couldn&apos;t Run Query:&lt;br&gt;&lt;br&gt;&quot; . $query . &quot;&lt;br&gt;&lt;br&gt;Error Code:&#xA0; &quot;.odbc_error());
&#xA0; &#xA0; &#xA0; unset($this-&gt;temp_fieldnames);

&#xA0; &#xA0; &#xA0; $i = 0;
&#xA0; &#xA0; &#xA0; $j = 0;
&#xA0; &#xA0; &#xA0; $num_rows = 0;

&#xA0; &#xA0; &#xA0; // only populate select queries
&#xA0; &#xA0; &#xA0; if (stripos($query, &apos;select &apos;) !== false) {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; while(odbc_fetch_row($res)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $num_rows++;
&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //Build tempory
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for ($j = 1; $j &lt;= odbc_num_fields($res); $j++) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $field_name = odbc_field_name($res, $j);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;temp_fieldnames[$j] = $field_name;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $ar[$field_name] = odbc_result($res, $field_name) . &quot;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $thisData[$i] = $ar;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i++;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; //populate the recordset and return it
&#xA0; &#xA0; &#xA0; $newRS-&gt;SetData( $thisData, $num_rows, $query );
&#xA0; &#xA0; &#xA0; return $newRS;
&#xA0;&#xA0; }
}
%&gt;

usage is pretty simple:

&lt;%
&#xA0; $con = new odbcConnection
&#xA0; $con-&gt;open(&quot;dsn&quot;,&quot;user&quot;,&quot;pass&quot;)

&#xA0; $sql = &quot;select bar from foo&quot;;
&#xA0; $rs = $con-&gt;execute($sql);

&#xA0; if (!$rs-&gt;eof) {
&#xA0; &#xA0; print $rs-&gt;data(&quot;bar&quot;);
&#xA0; &#xA0; &#xA0; // or //
&#xA0; &#xA0; print $rs-&gt;bar;
&#xA0; }

&#xA0; while (!$rs-&gt;eof) {
&#xA0; &#xA0; // blah blah code
&#xA0; &#xA0; $rs-&gt;movenext();
&#xA0; }
%&gt;

Works pretty well, but I haven&apos;t thoughly tested it yet.
Code can be dl&apos;d here:

http://www.russprince.com/odbc_functions.zip

Cheers,
Russ

  

#

[Official documentation page](https://www.php.net/manual/en/function.odbc-fetch-array.php)

**[To root](/README.md)**