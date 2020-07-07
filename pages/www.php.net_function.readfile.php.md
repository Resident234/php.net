# readfile





Just a note for those who face problems on names containing spaces (e.g. &quot;test test.pdf&quot;).

In the examples (99% of the time) you can find
header(&apos;Content-Disposition: attachment; filename=&apos;.basename($file));

but the correct way to set the filename is quoting it (double quote):
header(&apos;Content-Disposition: attachment; filename=&quot;&apos;.basename($file).&apos;&quot;&apos; );

Some browsers may work without quotation, but for sure not Firefox and as Mozilla explains, the quotation of the filename in the content-disposition is according to the RFC
http://kb.mozillazine.org/Filenames_with_spaces_are_truncated_upon_download

  

#



if you need to limit download rate, use this code 



```
<?php
$local_file = &apos;file.zip&apos;;
$download_file = &apos;name.zip&apos;;

// set the download rate limit (=&gt; 20,5 kb/s)
$download_rate = 20.5;
if(file_exists($local_file) &amp;&amp; is_file($local_file))
{
&#xA0; &#xA0; header(&apos;Cache-control: private&apos;);
&#xA0; &#xA0; header(&apos;Content-Type: application/octet-stream&apos;);
&#xA0; &#xA0; header(&apos;Content-Length: &apos;.filesize($local_file));
&#xA0; &#xA0; header(&apos;Content-Disposition: filename=&apos;.$download_file);

&#xA0; &#xA0; flush();
&#xA0; &#xA0; $file = fopen($local_file, &quot;r&quot;);
&#xA0; &#xA0; while(!feof($file))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // send the current file part to the browser
&#xA0; &#xA0; &#xA0; &#xA0; print fread($file, round($download_rate * 1024));
&#xA0; &#xA0; &#xA0; &#xA0; // flush the content to the browser
&#xA0; &#xA0; &#xA0; &#xA0; flush();
&#xA0; &#xA0; &#xA0; &#xA0; // sleep one second
&#xA0; &#xA0; &#xA0; &#xA0; sleep(1);
&#xA0; &#xA0; }
&#xA0; &#xA0; fclose($file);}
else {
&#xA0; &#xA0; die(&apos;Error: The file &apos;.$local_file.&apos; does not exist!&apos;);
}

?>
```



  

#



regarding php5:
i found out that there is already a disscussion @php-dev&#xA0; about readfile() and fpassthru() where only exactly 2 MB will be delivered.

so you may use this on php5 to get lager files


```
<?php
function readfile_chunked($filename,$retbytes=true) {
&#xA0; &#xA0; $chunksize = 1*(1024*1024); // how many bytes per chunk
&#xA0; &#xA0; $buffer = &apos;&apos;;
&#xA0; &#xA0; $cnt =0;
&#xA0; &#xA0; // $handle = fopen($filename, &apos;rb&apos;);
&#xA0; &#xA0; $handle = fopen($filename, &apos;rb&apos;);
&#xA0; &#xA0; if ($handle === false) {
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; while (!feof($handle)) {
&#xA0; &#xA0; &#xA0; &#xA0; $buffer = fread($handle, $chunksize);
&#xA0; &#xA0; &#xA0; &#xA0; echo $buffer;
&#xA0; &#xA0; &#xA0; &#xA0; if ($retbytes) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cnt += strlen($buffer);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $status = fclose($handle);
&#xA0; &#xA0; if ($retbytes &amp;&amp; $status) {
&#xA0; &#xA0; &#xA0; &#xA0; return $cnt; // return num. bytes delivered like readfile() does.
&#xA0; &#xA0; } 
&#xA0; &#xA0; return $status;

} 
?>
```



  

#



My script working correctly on IE6 and Firefox 2 with any typ e of files (I hope :))

function DownloadFile($file) { // $file = include path 
&#xA0; &#xA0; &#xA0; &#xA0; if(file_exists($file)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Content-Description: File Transfer&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Content-Type: application/octet-stream&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Content-Disposition: attachment; filename=&apos;.basename($file));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Content-Transfer-Encoding: binary&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Expires: 0&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Cache-Control: must-revalidate, post-check=0, pre-check=0&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Pragma: public&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Content-Length: &apos; . filesize($file));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ob_clean();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; flush();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; readfile($file);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; exit;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

Run on Apache 2 (WIN32) PHP5

  

#



To anyone that&apos;s had problems with Readfile() reading large files into memory the problem is not Readfile() itself, it&apos;s because you have output buffering on. Just turn off output buffering immediately before the call to Readfile(). Use something like ob_end_flush().

  

#

[Official documentation page](https://www.php.net/manual/en/function.readfile.php)

**[To root](/README.md)**