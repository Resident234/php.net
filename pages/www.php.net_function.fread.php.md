# fread



I couldn&apos;t get some of the previous resume scripts to work with Free Download Manager or Firefox.  I did some clean up and modified the code a little.<br><br>Changes:<br>1. Added a Flag to specify if you want download to be resumable or not<br>2. Some error checking and data cleanup for invalid/multiple ranges based on http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt<br>3. Always calculate a $seek_end even though the range specification says it could be empty... eg: bytes 500-/1234<br>4. Removed some Cache headers that didn&apos;t seem to be needed. (add back if you have problems)<br>5. Only send partial content header if downloading a piece of the file (IE workaround)<br><br>

```
<?php

function dl_file_resumable($file, $is_resume=TRUE)
{
    //First, see if the file exists
    if (!is_file($file))
    {
        die("<b>404 File not found!</b>");
    }

    //Gather relevent info about file
    $size = filesize($file);
    $fileinfo = pathinfo($file);
    
    //workaround for IE filename bug with multiple periods / multiple dots in filename
    //that adds square brackets to filename - eg. setup.abc.exe becomes setup[1].abc.exe
    $filename = (strstr($_SERVER['HTTP_USER_AGENT'], 'MSIE')) ?
                  preg_replace('/\./', '%2e', $fileinfo['basename'], substr_count($fileinfo['basename'], '.') - 1) :
                  $fileinfo['basename'];
    
    $file_extension = strtolower($path_info['extension']);

    //This will set the Content-Type to the appropriate setting for the file
    switch($file_extension)
    {
        case 'exe': $ctype='application/octet-stream'; break;
        case 'zip': $ctype='application/zip'; break;
        case 'mp3': $ctype='audio/mpeg'; break;
        case 'mpg': $ctype='video/mpeg'; break;
        case 'avi': $ctype='video/x-msvideo'; break;
        default:    $ctype='application/force-download';
    }

    //check if http_range is sent by browser (or download manager)
    if($is_resume &amp;&amp; isset($_SERVER['HTTP_RANGE']))
    {
        list($size_unit, $range_orig) = explode('=', $_SERVER['HTTP_RANGE'], 2);

        if ($size_unit == 'bytes')
        {
            //multiple ranges could be specified at the same time, but for simplicity only serve the first range
            //http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt
            list($range, $extra_ranges) = explode(',', $range_orig, 2);
        }
        else
        {
            $range = '';
        }
    }
    else
    {
        $range = '';
    }

    //figure out download piece from range (if set)
    list($seek_start, $seek_end) = explode('-', $range, 2);

    //set start and end based on range (if set), else set defaults
    //also check for invalid ranges.
    $seek_end = (empty($seek_end)) ? ($size - 1) : min(abs(intval($seek_end)),($size - 1));
    $seek_start = (empty($seek_start) || $seek_end < abs(intval($seek_start))) ? 0 : max(abs(intval($seek_start)),0);

    //add headers if resumable
    if ($is_resume)
    {
        //Only send partial content header if downloading a piece of the file (IE workaround)
        if ($seek_start > 0 || $seek_end < ($size - 1))
        {
            header('HTTP/1.1 206 Partial Content');
        }

        header('Accept-Ranges: bytes');
        header('Content-Range: bytes '.$seek_start.'-'.$seek_end.'/'.$size);
    }

    //headers for IE Bugs (is this necessary?)
    //header("Cache-Control: cache, must-revalidate");   
    //header("Pragma: public");

    header('Content-Type: ' . $ctype);
    header('Content-Disposition: attachment; filename="' . $filename . '"');
    header('Content-Length: '.($seek_end - $seek_start + 1));

    //open the file
    $fp = fopen($file, 'rb');
    //seek to start of missing part
    fseek($fp, $seek_start);

    //start buffered download
    while(!feof($fp))
    {
        //reset time limit for big files
        set_time_limit(0);
        print(fread($fp, 1024*8));
        flush();
        ob_flush();
    }

    fclose($fp);
    exit;
}

?>
```
  

#

This is an hack I&apos;ve done to download remote files with HTTP resume support. This is useful if you want to write a download script that fetches files remotely and then sends them to the user, adding support to download managers (I tested it on wget). To do that you should also use a "remote_filesize" function that you can easily write/find.<br><br>

```
<?php
function readfile_chunked_remote($filename, $seek = 0, $retbytes = true, $timeout = 3) {
    set_time_limit(0);
    $defaultchunksize = 1024*1024;
    $chunksize = $defaultchunksize;
    $buffer = '';
    $cnt = 0;
    $remotereadfile = false;

    if (preg_match('/[a-zA-Z]+:\/\//', $filename))
        $remotereadfile = true;

    $handle = @fopen($filename, 'rb');

    if ($handle === false) {
        return false;
    }

    stream_set_timeout($handle, $timeout);
    
    if ($seek != 0 &amp;&amp; !$remotereadfile)
        fseek($handle, $seek);

    while (!feof($handle)) {

        if ($remotereadfile &amp;&amp; $seek != 0 &amp;&amp; $cnt+$chunksize > $seek)
            $chunksize = $seek-$cnt;
        else
            $chunksize = $defaultchunksize;

        $buffer = @fread($handle, $chunksize);

        if ($retbytes || ($remotereadfile &amp;&amp; $seek != 0)) {
            $cnt += strlen($buffer);
        }

        if (!$remotereadfile || ($remotereadfile &amp;&amp; $cnt > $seek))
            echo $buffer;

        ob_flush();
        flush();
    }

    $info = stream_get_meta_data($handle);

    $status = fclose($handle);

    if ($info['timed_out'])
        return false;

    if ($retbytes &amp;&amp; $status) {
        return $cnt;
    }

    return $status;
}
?>
```
  

#

I had a fread script that hanged forever (from php manual):<br><br>

```
<?php
$fp = fsockopen("example.host.com", 80);
if (!$fp) {
    echo "$errstr ($errno)<br />\n";
} else {
    fwrite($fp, "Data sent by socket");
    $content = "";
    while (!feof($fp)) {  //This looped forever
        $content .= fread($fp, 1024);
    }
    fclose($fp);
    echo $content;
}
?>
```


The problem is that sometimes end of streaming is not marked by EOF nor a fixed mark, that's why this looped forever. This caused me a lot of headaches...
I solved it using the stream_get_meta_data function and a break statement as the following shows:



```
<?php
$fp = fsockopen("example.host.com", 80);
if (!$fp) {
    echo "$errstr ($errno)<br />\n";
} else {
    fwrite($fp, "Data sent by socket");
    $content = "";
    while (!feof($fp)) {  
        $content .= fread($fp, 1024);
        $stream_meta_data = stream_get_meta_data($fp); //Added line
         if($stream_meta_data['unread_bytes'] <= 0) break; //Added line
    }
    fclose($fp);
    echo $content;
}
?>
```
<br><br>Hope this will save a lot of headaches to someone.<br><br>(Greetings, from La Paz-Bolivia)  

#

For anyone still trying to write an effective file downloader function/script, the work has been done for you in all the major servers including Apache &amp; nginx.<br><br>Using the X-Sendfile header, you can do the following:<br><br>if ($user-&gt;isLoggedIn())<br>{<br>    header("X-Sendfile: $path_to_somefile_private");<br>    header("Content-Type: application/octet-stream");<br>    header("Content-Disposition: attachment; filename=\"$somefile\"");<br>}<br><br>Apache will serve the file for you while NOT revealing your private file path! Pretty nice. This works on all browsers/download managers and saves a lot of resources.<br><br>Documentation:<br>Apache module: https://tn123.org/mod_xsendfile/<br>Nginx: http://wiki.nginx.org/XSendfile<br>Lighttpd: http://blog.lighttpd.net/articles/2006/07/02/x-sendfile/<br><br>Hopefully this will save you many hours of work.  

#

[Official documentation page](https://www.php.net/manual/en/function.fread.php)

**[To root](/README.md)**