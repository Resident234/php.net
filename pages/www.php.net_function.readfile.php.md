# readfile



Just a note for those who face problems on names containing spaces (e.g. "test test.pdf").<br><br>In the examples (99% of the time) you can find<br>header(&apos;Content-Disposition: attachment; filename=&apos;.basename($file));<br><br>but the correct way to set the filename is quoting it (double quote):<br>header(&apos;Content-Disposition: attachment; filename="&apos;.basename($file).&apos;"&apos; );<br><br>Some browsers may work without quotation, but for sure not Firefox and as Mozilla explains, the quotation of the filename in the content-disposition is according to the RFC<br>http://kb.mozillazine.org/Filenames_with_spaces_are_truncated_upon_download  

#

if you need to limit download rate, use this code <br><br>

```
<?php
$local_file = 'file.zip';
$download_file = 'name.zip';

// set the download rate limit (=> 20,5 kb/s)
$download_rate = 20.5;
if(file_exists($local_file) &amp;&amp; is_file($local_file))
{
    header('Cache-control: private');
    header('Content-Type: application/octet-stream');
    header('Content-Length: '.filesize($local_file));
    header('Content-Disposition: filename='.$download_file);

    flush();
    $file = fopen($local_file, "r");
    while(!feof($file))
    {
        // send the current file part to the browser
        print fread($file, round($download_rate * 1024));
        // flush the content to the browser
        flush();
        // sleep one second
        sleep(1);
    }
    fclose($file);}
else {
    die('Error: The file '.$local_file.' does not exist!');
}

?>
```
  

#

regarding php5:<br>i found out that there is already a disscussion @php-dev  about readfile() and fpassthru() where only exactly 2 MB will be delivered.<br><br>so you may use this on php5 to get lager files<br>

```
<?php
function readfile_chunked($filename,$retbytes=true) {
    $chunksize = 1*(1024*1024); // how many bytes per chunk
    $buffer = '';
    $cnt =0;
    // $handle = fopen($filename, 'rb');
    $handle = fopen($filename, 'rb');
    if ($handle === false) {
        return false;
    }
    while (!feof($handle)) {
        $buffer = fread($handle, $chunksize);
        echo $buffer;
        if ($retbytes) {
            $cnt += strlen($buffer);
        }
    }
        $status = fclose($handle);
    if ($retbytes &amp;&amp; $status) {
        return $cnt; // return num. bytes delivered like readfile() does.
    } 
    return $status;

} 
?>
```
  

#

My script working correctly on IE6 and Firefox 2 with any typ e of files (I hope :))<br><br>function DownloadFile($file) { // $file = include path <br>        if(file_exists($file)) {<br>            header(&apos;Content-Description: File Transfer&apos;);<br>            header(&apos;Content-Type: application/octet-stream&apos;);<br>            header(&apos;Content-Disposition: attachment; filename=&apos;.basename($file));<br>            header(&apos;Content-Transfer-Encoding: binary&apos;);<br>            header(&apos;Expires: 0&apos;);<br>            header(&apos;Cache-Control: must-revalidate, post-check=0, pre-check=0&apos;);<br>            header(&apos;Pragma: public&apos;);<br>            header(&apos;Content-Length: &apos; . filesize($file));<br>            ob_clean();<br>            flush();<br>            readfile($file);<br>            exit;<br>        }<br><br>    }<br><br>Run on Apache 2 (WIN32) PHP5  

#

To anyone that&apos;s had problems with Readfile() reading large files into memory the problem is not Readfile() itself, it&apos;s because you have output buffering on. Just turn off output buffering immediately before the call to Readfile(). Use something like ob_end_flush().  

#

[Official documentation page](https://www.php.net/manual/en/function.readfile.php)

**[To root](/README.md)**