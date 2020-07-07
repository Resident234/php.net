# fputcsv





If you need to send a CSV file directly to the browser, without writing in an external file, you can open the output and use fputcsv on it..



```
<?php
$out = fopen(&apos;php://output&apos;, &apos;w&apos;);
fputcsv($out, array(&apos;this&apos;,&apos;is some&apos;, &apos;csv &quot;stuff&quot;, you know.&apos;));
fclose($out);
?>
```



  

#



Sometimes it&apos;s useful to get CSV line as string. I.e. to store it somewhere, not in on a filesystem.



```
<?php
function csvstr(array $fields) : string
{
&#xA0; &#xA0; $f = fopen(&apos;php://memory&apos;, &apos;r+&apos;);
&#xA0; &#xA0; if (fputcsv($f, $fields) === false) {
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; rewind($f);
&#xA0; &#xA0; $csv_line = stream_get_contents($f);
&#xA0; &#xA0; return rtrim($csv_line);
}
?>
```



  

#



If you need to save the output to a variable (e.g. for use within a framework) you can write to a temporary memory-wrapper and retrieve it&apos;s contents:



```
<?php
// output up to 5MB is kept in memory, if it becomes bigger it will automatically be written to a temporary file
$csv = fopen(&apos;php://temp/maxmemory:&apos;. (5*1024*1024), &apos;r+&apos;);

fputcsv($csv, array(&apos;blah&apos;,&apos;blah&apos;));

rewind($csv);

// put it all in a variable
$output = stream_get_contents($csv);
?>
```



  

#



if you want make UTF-8 file for excel, use this:

$fp = fopen($filename, &apos;w&apos;);
//add BOM to fix UTF-8 in Excel
fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));

  

#



TAB delimiting.

Using fputcsv to output a CSV with a tab delimiter is a little tricky since the delimiter field only takes one character.
The answer is to use the chr() function.&#xA0; The ascii code for tab is 9, so chr(9) returns a tab character.



```
<?php
&#xA0; &#xA0; fputcsv($fp, $foo, &apos;\t&apos;);&#xA0; &#xA0; &#xA0; //won&apos;t work
&#xA0; &#xA0; fputcsv($fp, $foo, &apos;&#xA0; &#xA0; &apos;);&#xA0; &#xA0; //won&apos;t work

&#xA0; &#xA0; fputcsv($fp, $foo, chr(9));&#xA0; &#xA0; //works
?>
```


==================

it should be:


```
<?php
&#xA0; &#xA0; fputcsv($fp, $foo, &quot;\t&quot;);
?>
```

you just forgot that single quotes are literal...meaning whatever you put there that&apos;s what will come out so &apos;\t&apos; would be same as &apos;t&apos; because \ in that case would be only used for escaping but if you use double quotes then that would work.

  

#



I&apos;ve created a function for quickly generating CSV files that work with Microsoft applications. In the field I learned a few things about generating CSVs that are not always obvious. First, since PHP is generally *nix-based, it makes sense that the line endings are always \n instead of \r\n. However, certain Microsoft programs (I&apos;m looking at you, Access 97), will fail to recognize the CSV properly unless each line ends with \r\n. So this function changes the line endings accordingly. Secondly, if the first column heading / value of the CSV file begins with uppercase ID, certain Microsoft programs (ahem, Excel 2007) will interpret the file as being in the SYLK format rather than CSV, as described here: http://support.microsoft.com/kb/323626



This function accommodates for that as well, by forcibly enclosing that first value in quotes (when this doesn&apos;t occur automatically). It would be fairly simple to modify this function to use another delimiter if need be and I leave that as an exercise to the reader. So quite simply, this function is used for outputting CSV data to a CSV file in a way that is safe for use with Windows applications. It takes two parameters + one optional parameter: the location of where the file should be saved, an array of data rows, and an optional array of column headings. (Technically you could omit the headings array and just include it as the first row of the data, but it is often useful to keep this data stored in different arrays in practice.)





```
<?php



function mssafe_csv($filepath, $data, $header = array())

{

&#xA0; &#xA0; if ( $fp = fopen($filepath, &apos;w&apos;) ) {

&#xA0; &#xA0; &#xA0; &#xA0; $show_header = true;

&#xA0; &#xA0; &#xA0; &#xA0; if ( empty($header) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $show_header = false;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; reset($data);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line = current($data);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( !empty($line) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; reset($line);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $first = current($line);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( substr($first, 0, 2) == &apos;ID&apos; &amp;&amp; !preg_match(&apos;/[&quot;\\s,]/&apos;, $first) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_shift($data);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_shift($line);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( empty($line) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\&quot;{$first}\&quot;\r\n&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\&quot;{$first}\&quot;,&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fputcsv($fp, $line);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($fp, -1, SEEK_CUR);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\r\n&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; reset($header);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $first = current($header);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( substr($first, 0, 2) == &apos;ID&apos; &amp;&amp; !preg_match(&apos;/[&quot;\\s,]/&apos;, $first) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_shift($header);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( empty($header) ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $show_header = false;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\&quot;{$first}\&quot;\r\n&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\&quot;{$first}\&quot;,&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; if ( $show_header ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fputcsv($fp, $header);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($fp, -1, SEEK_CUR);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\r\n&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; foreach ( $data as $line ) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fputcsv($fp, $line);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($fp, -1, SEEK_CUR);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fp, &quot;\r\n&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; fclose($fp);

&#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; }

&#xA0; &#xA0; return true;

}



?>
```



  

#



Utility function to output a mysql query to csv with the option to write to file or send back to the browser as a csv attachment.



```
<?php
&#xA0; &#xA0; function query_to_csv($db_conn, $query, $filename, $attachment = false, $headers = true) {
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if($attachment) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // send response headers to the browser
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header( &apos;Content-Type: text/csv&apos; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header( &apos;Content-Disposition: attachment;filename=&apos;.$filename);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fp = fopen(&apos;php://output&apos;, &apos;w&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fp = fopen($filename, &apos;w&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $result = mysql_query($query, $db_conn) or die( mysql_error( $db_conn ) );
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if($headers) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // output header row (if at least one row exists)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $row = mysql_fetch_assoc($result);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($row) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fputcsv($fp, array_keys($row));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // reset pointer back to beginning
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; mysql_data_seek($result, 0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; while($row = mysql_fetch_assoc($result)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fputcsv($fp, $row);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; }

&#xA0; &#xA0; // Using the function
&#xA0; &#xA0; $sql = &quot;SELECT * FROM table&quot;;
&#xA0; &#xA0; // $db_conn should be a valid db handle

&#xA0; &#xA0; // output as an attachment
&#xA0; &#xA0; query_to_csv($db_conn, $sql, &quot;test.csv&quot;, true);

&#xA0; &#xA0; // output to file system
&#xA0; &#xA0; query_to_csv($db_conn, $sql, &quot;test.csv&quot;, false);
?>
```



  

#



Alright, after playing a while, I&apos;m confident the following replacement function works in all cases, including the ones for which the native fputcsv function fails. If fputcsv fails to work for you (particularly with mysql csv imports), try this function as a drop-in replacement instead.



Arguments to pass in are exactly the same as for fputcsv, though I have added an additional $mysql_null boolean which allows one to turn php null&apos;s into mysql-insertable nulls (by default, this add-on is disabled, thus working identically to fputcsv [except this one works!]).





```
<?php



function fputcsv2 ($fh, array $fields, $delimiter = &apos;,&apos;, $enclosure = &apos;&quot;&apos;, $mysql_null = false) {

&#xA0; &#xA0; $delimiter_esc = preg_quote($delimiter, &apos;/&apos;);

&#xA0; &#xA0; $enclosure_esc = preg_quote($enclosure, &apos;/&apos;);



&#xA0; &#xA0; $output = array();

&#xA0; &#xA0; foreach ($fields as $field) {

&#xA0; &#xA0; &#xA0; &#xA0; if ($field === null &amp;&amp; $mysql_null) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $output[] = &apos;NULL&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;

&#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; $output[] = preg_match(&quot;/(?:${delimiter_esc}|${enclosure_esc}|\s)/&quot;, $field) ? (

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $enclosure . str_replace($enclosure, $enclosure . $enclosure, $field) . $enclosure

&#xA0; &#xA0; &#xA0; &#xA0; ) : $field;

&#xA0; &#xA0; }



&#xA0; &#xA0; fwrite($fh, join($delimiter, $output) . &quot;\n&quot;);

}



// the _EXACT_ LOAD DATA INFILE command to use

// (if you pass in something different for $delimiter

// and/or $enclosure above, change them here too;

// but _LEAVE ESCAPED BY EMPTY!_).

/*

LOAD DATA INFILE

&#xA0; &#xA0; &apos;/path/to/file.csv&apos;



INTO TABLE

&#xA0; &#xA0; my_table



FIELDS TERMINATED BY

&#xA0; &#xA0; &apos;,&apos;



OPTIONALLY ENCLOSED BY

&#xA0; &#xA0; &apos;&quot;&apos;



ESCAPED BY

&#xA0; &#xA0; &apos;&apos;



LINES TERMINATED BY

&#xA0; &#xA0; &apos;\n&apos;

*/



?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.fputcsv.php)

**[To root](/README.md)**