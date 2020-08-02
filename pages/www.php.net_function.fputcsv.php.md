# fputcsv



If you need to send a CSV file directly to the browser, without writing in an external file, you can open the output and use fputcsv on it..<br><br>

```
<?php
$out = fopen('php://output', 'w');
fputcsv($out, array('this','is some', 'csv "stuff", you know.'));
fclose($out);
?>
```
  

---

If you need to save the output to a variable (e.g. for use within a framework) you can write to a temporary memory-wrapper and retrieve it&apos;s contents:<br><br>

```
<?php
// output up to 5MB is kept in memory, if it becomes bigger it will automatically be written to a temporary file
$csv = fopen('php://temp/maxmemory:'. (5*1024*1024), 'r+');

fputcsv($csv, array('blah','blah'));

rewind($csv);

// put it all in a variable
$output = stream_get_contents($csv);
?>
```
  

---

Sometimes it&apos;s useful to get CSV line as string. I.e. to store it somewhere, not in on a filesystem.<br><br>

```
<?php
function csvstr(array $fields) : string
{
    $f = fopen('php://memory', 'r+');
    if (fputcsv($f, $fields) === false) {
        return false;
    }
    rewind($f);
    $csv_line = stream_get_contents($f);
    return rtrim($csv_line);
}
?>
```
  

---

if you want make UTF-8 file for excel, use this:<br><br>$fp = fopen($filename, &apos;w&apos;);<br>//add BOM to fix UTF-8 in Excel<br>fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));  

---

TAB delimiting.<br><br>Using fputcsv to output a CSV with a tab delimiter is a little tricky since the delimiter field only takes one character.<br>The answer is to use the chr() function.  The ascii code for tab is 9, so chr(9) returns a tab character.<br><br>

```
<?php
    fputcsv($fp, $foo, '\t');      //won't work
    fputcsv($fp, $foo, '    ');    //won't work

    fputcsv($fp, $foo, chr(9));    //works
?>
```


==================

it should be:


```
<?php
    fputcsv($fp, $foo, "\t");
?>
```
<br>you just forgot that single quotes are literal...meaning whatever you put there that&apos;s what will come out so &apos;\t&apos; would be same as &apos;t&apos; because \ in that case would be only used for escaping but if you use double quotes then that would work.  

---

I&apos;ve created a function for quickly generating CSV files that work with Microsoft applications. In the field I learned a few things about generating CSVs that are not always obvious. First, since PHP is generally *nix-based, it makes sense that the line endings are always \n instead of \r\n. However, certain Microsoft programs (I&apos;m looking at you, Access 97), will fail to recognize the CSV properly unless each line ends with \r\n. So this function changes the line endings accordingly. Secondly, if the first column heading / value of the CSV file begins with uppercase ID, certain Microsoft programs (ahem, Excel 2007) will interpret the file as being in the SYLK format rather than CSV, as described here: http://support.microsoft.com/kb/323626<br><br>This function accommodates for that as well, by forcibly enclosing that first value in quotes (when this doesn&apos;t occur automatically). It would be fairly simple to modify this function to use another delimiter if need be and I leave that as an exercise to the reader. So quite simply, this function is used for outputting CSV data to a CSV file in a way that is safe for use with Windows applications. It takes two parameters + one optional parameter: the location of where the file should be saved, an array of data rows, and an optional array of column headings. (Technically you could omit the headings array and just include it as the first row of the data, but it is often useful to keep this data stored in different arrays in practice.)<br><br>

```
<?php

function mssafe_csv($filepath, $data, $header = array())
{
    if ( $fp = fopen($filepath, 'w') ) {
        $show_header = true;
        if ( empty($header) ) {
            $show_header = false;
            reset($data);
            $line = current($data);
            if ( !empty($line) ) {
                reset($line);
                $first = current($line);
                if ( substr($first, 0, 2) == 'ID' &amp;&amp; !preg_match('/["\\s,]/', $first) ) {
                    array_shift($data);
                    array_shift($line);
                    if ( empty($line) ) {
                        fwrite($fp, "\"{$first}\"\r\n");
                    } else {
                        fwrite($fp, "\"{$first}\",");
                        fputcsv($fp, $line);
                        fseek($fp, -1, SEEK_CUR);
                        fwrite($fp, "\r\n");
                    }
                }
            }
        } else {
            reset($header);
            $first = current($header);
            if ( substr($first, 0, 2) == 'ID' &amp;&amp; !preg_match('/["\\s,]/', $first) ) {
                array_shift($header);
                if ( empty($header) ) {
                    $show_header = false;
                    fwrite($fp, "\"{$first}\"\r\n");
                } else {
                    fwrite($fp, "\"{$first}\",");
                }
            }
        }
        if ( $show_header ) {
            fputcsv($fp, $header);
            fseek($fp, -1, SEEK_CUR);
            fwrite($fp, "\r\n");
        }
        foreach ( $data as $line ) {
            fputcsv($fp, $line);
            fseek($fp, -1, SEEK_CUR);
            fwrite($fp, "\r\n");
        }
        fclose($fp);
    } else {
        return false;
    }
    return true;
}

?>
```
  

---

Utility function to output a mysql query to csv with the option to write to file or send back to the browser as a csv attachment.<br><br>

```
<?php
    function query_to_csv($db_conn, $query, $filename, $attachment = false, $headers = true) {
        
        if($attachment) {
            // send response headers to the browser
            header( 'Content-Type: text/csv' );
            header( 'Content-Disposition: attachment;filename='.$filename);
            $fp = fopen('php://output', 'w');
        } else {
            $fp = fopen($filename, 'w');
        }
        
        $result = mysql_query($query, $db_conn) or die( mysql_error( $db_conn ) );
        
        if($headers) {
            // output header row (if at least one row exists)
            $row = mysql_fetch_assoc($result);
            if($row) {
                fputcsv($fp, array_keys($row));
                // reset pointer back to beginning
                mysql_data_seek($result, 0);
            }
        }
        
        while($row = mysql_fetch_assoc($result)) {
            fputcsv($fp, $row);
        }
        
        fclose($fp);
    }

    // Using the function
    $sql = "SELECT * FROM table";
    // $db_conn should be a valid db handle

    // output as an attachment
    query_to_csv($db_conn, $sql, "test.csv", true);

    // output to file system
    query_to_csv($db_conn, $sql, "test.csv", false);
?>
```
  

---

Alright, after playing a while, I&apos;m confident the following replacement function works in all cases, including the ones for which the native fputcsv function fails. If fputcsv fails to work for you (particularly with mysql csv imports), try this function as a drop-in replacement instead.<br><br>Arguments to pass in are exactly the same as for fputcsv, though I have added an additional $mysql_null boolean which allows one to turn php null&apos;s into mysql-insertable nulls (by default, this add-on is disabled, thus working identically to fputcsv [except this one works!]).<br><br>

```
<?php

function fputcsv2 ($fh, array $fields, $delimiter = ',', $enclosure = '"', $mysql_null = false) {
    $delimiter_esc = preg_quote($delimiter, '/');
    $enclosure_esc = preg_quote($enclosure, '/');

    $output = array();
    foreach ($fields as $field) {
        if ($field === null &amp;&amp; $mysql_null) {
            $output[] = 'NULL';
            continue;
        }

        $output[] = preg_match("/(?:${delimiter_esc}|${enclosure_esc}|\s)/", $field) ? (
            $enclosure . str_replace($enclosure, $enclosure . $enclosure, $field) . $enclosure
        ) : $field;
    }

    fwrite($fh, join($delimiter, $output) . "\n");
}

// the _EXACT_ LOAD DATA INFILE command to use
// (if you pass in something different for $delimiter
// and/or $enclosure above, change them here too;
// but _LEAVE ESCAPED BY EMPTY!_).
/*
LOAD DATA INFILE
    '/path/to/file.csv'

INTO TABLE
    my_table

FIELDS TERMINATED BY
    ','

OPTIONALLY ENCLOSED BY
    '"'

ESCAPED BY
    ''

LINES TERMINATED BY
    '\n'
*/

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.fputcsv.php)

**[To root](/README.md)**