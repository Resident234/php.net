# fgetcsv



If you need to set auto_detect_line_endings to deal with Mac line endings, it may seem obvious but remember it should be set before fopen, not after:<br><br>This will work:<br>

```
<?php
ini_set('auto_detect_line_endings',TRUE);
$handle = fopen('/path/to/file','r');
while ( ($data = fgetcsv($handle) ) !== FALSE ) {
//process
}
ini_set('auto_detect_line_endings',FALSE);
?>
```


This won't, you will still get concatenated fields at the new line position:


```
<?php
$handle = fopen('/path/to/file','r');
ini_set('auto_detect_line_endings',TRUE);
while ( ($data = fgetcsv($handle) ) !== FALSE ) {
//process
}
ini_set('auto_detect_line_endings',FALSE);
?>
```
  

#

This function has no special BOM handling. The first cell of the first row will inherit the BOM bytes, i.e. will be 3 bytes longer than expected. As the BOM is invisible you may not notice.<br><br>Excel on Windows, or text editors like Notepad, may add the BOM.  

#

fgetcsv seems to handle newlines within fields fine. So in fact it is not reading a line, but keeps reading untill it finds a \n-character that&apos;s not quoted as a field.<br><br>Example:<br><br>

```
<?php
/* test.csv contains:
"col 1","col2","col3"
"this
is
having
multiple
lines","this not","this also not"
"normal record","nothing to see here","no data"
*/

$handle = fopen("test.csv", "r");
while (($data = fgetcsv($handle)) !== FALSE) {
    var_dump($data);
}
?>
```
<br><br>Returns:<br>array(3) {<br>  [0]=&gt;<br>  string(5) "col 1"<br>  [1]=&gt;<br>  string(4) "col2"<br>  [2]=&gt;<br>  string(4) "col3"<br>}<br>array(3) {<br>  [0]=&gt;<br>  string(29) "this<br>is<br>having<br>multiple<br>lines"<br>  [1]=&gt;<br>  string(8) "this not"<br>  [2]=&gt;<br>  string(13) "this also not"<br>}<br>array(3) {<br>  [0]=&gt;<br>  string(13) "normal record"<br>  [1]=&gt;<br>  string(19) "nothing to see here"<br>  [2]=&gt;<br>  string(7) "no data"<br>}<br><br>This means that you can expect fgetcsv to handle newlines within fields fine. This was not clear from the documentation.  

#

Here is a OOP based importer similar to the one posted earlier. However, this is slightly more flexible in that you can import huge files without running out of memory, you just have to use a limit on the get() method<br><br>Sample usage for small files:-<br>-------------------------------------<br>

```
<?php
$importer = new CsvImporter("small.txt",true);
$data = $importer->get();
print_r($data);
?>
```



Sample usage for large files:-
-------------------------------------


```
<?php
$importer = new CsvImporter("large.txt",true);
while($data = $importer->get(2000))
{
print_r($data);
}
?>
```



And heres the class:-
-------------------------------------


```
<?php
class CsvImporter
{
    private $fp;
    private $parse_header;
    private $header;
    private $delimiter;
    private $length;
    //--------------------------------------------------------------------
    function __construct($file_name, $parse_header=false, $delimiter="\t", $length=8000)
    {
        $this->fp = fopen($file_name, "r");
        $this->parse_header = $parse_header;
        $this->delimiter = $delimiter;
        $this->length = $length;
        $this->lines = $lines;

        if ($this->parse_header)
        {
           $this->header = fgetcsv($this->fp, $this->length, $this->delimiter);
        }

    }
    //--------------------------------------------------------------------
    function __destruct()
    {
        if ($this->fp)
        {
            fclose($this->fp);
        }
    }
    //--------------------------------------------------------------------
    function get($max_lines=0)
    {
        //if $max_lines is set to 0, then get all the data

        $data = array();

        if ($max_lines &gt; 0)
            $line_count = 0;
        else
            $line_count = -1; // so loop limit is ignored

        while ($line_count &lt; $max_lines &amp;&amp; ($row = fgetcsv($this->fp, $this->length, $this->delimiter)) !== FALSE)
        {
            if ($this->parse_header)
            {
                foreach ($this->header as $i => $heading_i)
                {
                    $row_new[$heading_i] = $row[$i];
                }
                $data[] = $row_new;
            }
            else
            {
                $data[] = $row;
            }

            if ($max_lines &gt; 0)
                $line_count++;
        }
        return $data;
    }
    //--------------------------------------------------------------------

}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.fgetcsv.php)

**[To root](/README.md)**