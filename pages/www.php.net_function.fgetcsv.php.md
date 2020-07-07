# fgetcsv





If you need to set auto_detect_line_endings to deal with Mac line endings, it may seem obvious but remember it should be set before fopen, not after:

This will work:


```
<?php
ini_set(&apos;auto_detect_line_endings&apos;,TRUE);
$handle = fopen(&apos;/path/to/file&apos;,&apos;r&apos;);
while ( ($data = fgetcsv($handle) ) !== FALSE ) {
//process
}
ini_set(&apos;auto_detect_line_endings&apos;,FALSE);
?>
```


This won&apos;t, you will still get concatenated fields at the new line position:


```
<?php
$handle = fopen(&apos;/path/to/file&apos;,&apos;r&apos;);
ini_set(&apos;auto_detect_line_endings&apos;,TRUE);
while ( ($data = fgetcsv($handle) ) !== FALSE ) {
//process
}
ini_set(&apos;auto_detect_line_endings&apos;,FALSE);
?>
```



  

#



This function has no special BOM handling. The first cell of the first row will inherit the BOM bytes, i.e. will be 3 bytes longer than expected. As the BOM is invisible you may not notice.

Excel on Windows, or text editors like Notepad, may add the BOM.

  

#



fgetcsv seems to handle newlines within fields fine. So in fact it is not reading a line, but keeps reading untill it finds a \n-character that&apos;s not quoted as a field.

Example:



```
<?php
/* test.csv contains:
&quot;col 1&quot;,&quot;col2&quot;,&quot;col3&quot;
&quot;this
is
having
multiple
lines&quot;,&quot;this not&quot;,&quot;this also not&quot;
&quot;normal record&quot;,&quot;nothing to see here&quot;,&quot;no data&quot;
*/

$handle = fopen(&quot;test.csv&quot;, &quot;r&quot;);
while (($data = fgetcsv($handle)) !== FALSE) {
&#xA0; &#xA0; var_dump($data);
}
?>
```


Returns:
array(3) {
&#xA0; [0]=&gt;
&#xA0; string(5) &quot;col 1&quot;
&#xA0; [1]=&gt;
&#xA0; string(4) &quot;col2&quot;
&#xA0; [2]=&gt;
&#xA0; string(4) &quot;col3&quot;
}
array(3) {
&#xA0; [0]=&gt;
&#xA0; string(29) &quot;this
is
having
multiple
lines&quot;
&#xA0; [1]=&gt;
&#xA0; string(8) &quot;this not&quot;
&#xA0; [2]=&gt;
&#xA0; string(13) &quot;this also not&quot;
}
array(3) {
&#xA0; [0]=&gt;
&#xA0; string(13) &quot;normal record&quot;
&#xA0; [1]=&gt;
&#xA0; string(19) &quot;nothing to see here&quot;
&#xA0; [2]=&gt;
&#xA0; string(7) &quot;no data&quot;
}

This means that you can expect fgetcsv to handle newlines within fields fine. This was not clear from the documentation.

  

#



Here is a OOP based importer similar to the one posted earlier. However, this is slightly more flexible in that you can import huge files without running out of memory, you just have to use a limit on the get() method



Sample usage for small files:-

-------------------------------------



```
<?php

$importer = new CsvImporter(&quot;small.txt&quot;,true);

$data = $importer-&gt;get();

print_r($data);

?>
```






Sample usage for large files:-

-------------------------------------



```
<?php

$importer = new CsvImporter(&quot;large.txt&quot;,true);

while($data = $importer-&gt;get(2000))

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

&#xA0; &#xA0; private $fp;

&#xA0; &#xA0; private $parse_header;

&#xA0; &#xA0; private $header;

&#xA0; &#xA0; private $delimiter;

&#xA0; &#xA0; private $length;

&#xA0; &#xA0; //--------------------------------------------------------------------

&#xA0; &#xA0; function __construct($file_name, $parse_header=false, $delimiter=&quot;\t&quot;, $length=8000)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;fp = fopen($file_name, &quot;r&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;parse_header = $parse_header;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;delimiter = $delimiter;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;length = $length;

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;lines = $lines;



&#xA0; &#xA0; &#xA0; &#xA0; if ($this-&gt;parse_header)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;header = fgetcsv($this-&gt;fp, $this-&gt;length, $this-&gt;delimiter);

&#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; }

&#xA0; &#xA0; //--------------------------------------------------------------------

&#xA0; &#xA0; function __destruct()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; if ($this-&gt;fp)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($this-&gt;fp);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; //--------------------------------------------------------------------

&#xA0; &#xA0; function get($max_lines=0)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; //if $max_lines is set to 0, then get all the data



&#xA0; &#xA0; &#xA0; &#xA0; $data = array();



&#xA0; &#xA0; &#xA0; &#xA0; if ($max_lines &gt; 0)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line_count = 0;

&#xA0; &#xA0; &#xA0; &#xA0; else

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line_count = -1; // so loop limit is ignored



&#xA0; &#xA0; &#xA0; &#xA0; while ($line_count &lt; $max_lines &amp;&amp; ($row = fgetcsv($this-&gt;fp, $this-&gt;length, $this-&gt;delimiter)) !== FALSE)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($this-&gt;parse_header)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($this-&gt;header as $i =&gt; $heading_i)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $row_new[$heading_i] = $row[$i];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $data[] = $row_new;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $data[] = $row;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($max_lines &gt; 0)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line_count++;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return $data;

&#xA0; &#xA0; }

&#xA0; &#xA0; //--------------------------------------------------------------------



}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.fgetcsv.php)

**[To root](/README.md)**