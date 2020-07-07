# fread





I couldn&apos;t get some of the previous resume scripts to work with Free Download Manager or Firefox.&#xA0; I did some clean up and modified the code a little.

Changes:
1. Added a Flag to specify if you want download to be resumable or not
2. Some error checking and data cleanup for invalid/multiple ranges based on http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt
3. Always calculate a $seek_end even though the range specification says it could be empty... eg: bytes 500-/1234
4. Removed some Cache headers that didn&apos;t seem to be needed. (add back if you have problems)
5. Only send partial content header if downloading a piece of the file (IE workaround)



```
<?php

function dl_file_resumable($file, $is_resume=TRUE)
{
&#xA0; &#xA0; //First, see if the file exists
&#xA0; &#xA0; if (!is_file($file))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;&lt;b&gt;404 File not found!&lt;/b&gt;&quot;);
&#xA0; &#xA0; }

&#xA0; &#xA0; //Gather relevent info about file
&#xA0; &#xA0; $size = filesize($file);
&#xA0; &#xA0; $fileinfo = pathinfo($file);
&#xA0; &#xA0; 
&#xA0; &#xA0; //workaround for IE filename bug with multiple periods / multiple dots in filename
&#xA0; &#xA0; //that adds square brackets to filename - eg. setup.abc.exe becomes setup[1].abc.exe
&#xA0; &#xA0; $filename = (strstr($_SERVER[&apos;HTTP_USER_AGENT&apos;], &apos;MSIE&apos;)) ?
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; preg_replace(&apos;/\./&apos;, &apos;%2e&apos;, $fileinfo[&apos;basename&apos;], substr_count($fileinfo[&apos;basename&apos;], &apos;.&apos;) - 1) :
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fileinfo[&apos;basename&apos;];
&#xA0; &#xA0; 
&#xA0; &#xA0; $file_extension = strtolower($path_info[&apos;extension&apos;]);

&#xA0; &#xA0; //This will set the Content-Type to the appropriate setting for the file
&#xA0; &#xA0; switch($file_extension)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;exe&apos;: $ctype=&apos;application/octet-stream&apos;; break;
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;zip&apos;: $ctype=&apos;application/zip&apos;; break;
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;mp3&apos;: $ctype=&apos;audio/mpeg&apos;; break;
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;mpg&apos;: $ctype=&apos;video/mpeg&apos;; break;
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;avi&apos;: $ctype=&apos;video/x-msvideo&apos;; break;
&#xA0; &#xA0; &#xA0; &#xA0; default:&#xA0; &#xA0; $ctype=&apos;application/force-download&apos;;
&#xA0; &#xA0; }

&#xA0; &#xA0; //check if http_range is sent by browser (or download manager)
&#xA0; &#xA0; if($is_resume &amp;&amp; isset($_SERVER[&apos;HTTP_RANGE&apos;]))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; list($size_unit, $range_orig) = explode(&apos;=&apos;, $_SERVER[&apos;HTTP_RANGE&apos;], 2);

&#xA0; &#xA0; &#xA0; &#xA0; if ($size_unit == &apos;bytes&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //multiple ranges could be specified at the same time, but for simplicity only serve the first range
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($range, $extra_ranges) = explode(&apos;,&apos;, $range_orig, 2);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $range = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; else
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $range = &apos;&apos;;
&#xA0; &#xA0; }

&#xA0; &#xA0; //figure out download piece from range (if set)
&#xA0; &#xA0; list($seek_start, $seek_end) = explode(&apos;-&apos;, $range, 2);

&#xA0; &#xA0; //set start and end based on range (if set), else set defaults
&#xA0; &#xA0; //also check for invalid ranges.
&#xA0; &#xA0; $seek_end = (empty($seek_end)) ? ($size - 1) : min(abs(intval($seek_end)),($size - 1));
&#xA0; &#xA0; $seek_start = (empty($seek_start) || $seek_end &lt; abs(intval($seek_start))) ? 0 : max(abs(intval($seek_start)),0);

&#xA0; &#xA0; //add headers if resumable
&#xA0; &#xA0; if ($is_resume)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; //Only send partial content header if downloading a piece of the file (IE workaround)
&#xA0; &#xA0; &#xA0; &#xA0; if ($seek_start &gt; 0 || $seek_end &lt; ($size - 1))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; header(&apos;HTTP/1.1 206 Partial Content&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Accept-Ranges: bytes&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; header(&apos;Content-Range: bytes &apos;.$seek_start.&apos;-&apos;.$seek_end.&apos;/&apos;.$size);
&#xA0; &#xA0; }

&#xA0; &#xA0; //headers for IE Bugs (is this necessary?)
&#xA0; &#xA0; //header(&quot;Cache-Control: cache, must-revalidate&quot;);&#xA0;&#xA0; 
&#xA0; &#xA0; //header(&quot;Pragma: public&quot;);

&#xA0; &#xA0; header(&apos;Content-Type: &apos; . $ctype);
&#xA0; &#xA0; header(&apos;Content-Disposition: attachment; filename=&quot;&apos; . $filename . &apos;&quot;&apos;);
&#xA0; &#xA0; header(&apos;Content-Length: &apos;.($seek_end - $seek_start + 1));

&#xA0; &#xA0; //open the file
&#xA0; &#xA0; $fp = fopen($file, &apos;rb&apos;);
&#xA0; &#xA0; //seek to start of missing part
&#xA0; &#xA0; fseek($fp, $seek_start);

&#xA0; &#xA0; //start buffered download
&#xA0; &#xA0; while(!feof($fp))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; //reset time limit for big files
&#xA0; &#xA0; &#xA0; &#xA0; set_time_limit(0);
&#xA0; &#xA0; &#xA0; &#xA0; print(fread($fp, 1024*8));
&#xA0; &#xA0; &#xA0; &#xA0; flush();
&#xA0; &#xA0; &#xA0; &#xA0; ob_flush();
&#xA0; &#xA0; }

&#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; exit;
}

?>
```



  

#



This is an hack I&apos;ve done to download remote files with HTTP resume support. This is useful if you want to write a download script that fetches files remotely and then sends them to the user, adding support to download managers (I tested it on wget). To do that you should also use a &quot;remote_filesize&quot; function that you can easily write/find.





```
<?php

function readfile_chunked_remote($filename, $seek = 0, $retbytes = true, $timeout = 3) {

&#xA0; &#xA0; set_time_limit(0);

&#xA0; &#xA0; $defaultchunksize = 1024*1024;

&#xA0; &#xA0; $chunksize = $defaultchunksize;

&#xA0; &#xA0; $buffer = &apos;&apos;;

&#xA0; &#xA0; $cnt = 0;

&#xA0; &#xA0; $remotereadfile = false;



&#xA0; &#xA0; if (preg_match(&apos;/[a-zA-Z]+:\/\//&apos;, $filename))

&#xA0; &#xA0; &#xA0; &#xA0; $remotereadfile = true;



&#xA0; &#xA0; $handle = @fopen($filename, &apos;rb&apos;);



&#xA0; &#xA0; if ($handle === false) {

&#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; }



&#xA0; &#xA0; stream_set_timeout($handle, $timeout);

&#xA0; &#xA0; 

&#xA0; &#xA0; if ($seek != 0 &amp;&amp; !$remotereadfile)

&#xA0; &#xA0; &#xA0; &#xA0; fseek($handle, $seek);



&#xA0; &#xA0; while (!feof($handle)) {



&#xA0; &#xA0; &#xA0; &#xA0; if ($remotereadfile &amp;&amp; $seek != 0 &amp;&amp; $cnt+$chunksize &gt; $seek)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $chunksize = $seek-$cnt;

&#xA0; &#xA0; &#xA0; &#xA0; else

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $chunksize = $defaultchunksize;



&#xA0; &#xA0; &#xA0; &#xA0; $buffer = @fread($handle, $chunksize);



&#xA0; &#xA0; &#xA0; &#xA0; if ($retbytes || ($remotereadfile &amp;&amp; $seek != 0)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cnt += strlen($buffer);

&#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; if (!$remotereadfile || ($remotereadfile &amp;&amp; $cnt &gt; $seek))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo $buffer;



&#xA0; &#xA0; &#xA0; &#xA0; ob_flush();

&#xA0; &#xA0; &#xA0; &#xA0; flush();

&#xA0; &#xA0; }



&#xA0; &#xA0; $info = stream_get_meta_data($handle);



&#xA0; &#xA0; $status = fclose($handle);



&#xA0; &#xA0; if ($info[&apos;timed_out&apos;])

&#xA0; &#xA0; &#xA0; &#xA0; return false;



&#xA0; &#xA0; if ($retbytes &amp;&amp; $status) {

&#xA0; &#xA0; &#xA0; &#xA0; return $cnt;

&#xA0; &#xA0; }



&#xA0; &#xA0; return $status;

}

?>
```



  

#



I had a fread script that hanged forever (from php manual):



```
<?php
$fp = fsockopen(&quot;example.host.com&quot;, 80);
if (!$fp) {
&#xA0; &#xA0; echo &quot;$errstr ($errno)&lt;br /&gt;\n&quot;;
} else {
&#xA0; &#xA0; fwrite($fp, &quot;Data sent by socket&quot;);
&#xA0; &#xA0; $content = &quot;&quot;;
&#xA0; &#xA0; while (!feof($fp)) {&#xA0; //This looped forever
&#xA0; &#xA0; &#xA0; &#xA0; $content .= fread($fp, 1024);
&#xA0; &#xA0; }
&#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; echo $content;
}
?>
```


The problem is that sometimes end of streaming is not marked by EOF nor a fixed mark, that&apos;s why this looped forever. This caused me a lot of headaches...
I solved it using the stream_get_meta_data function and a break statement as the following shows:



```
<?php
$fp = fsockopen(&quot;example.host.com&quot;, 80);
if (!$fp) {
&#xA0; &#xA0; echo &quot;$errstr ($errno)&lt;br /&gt;\n&quot;;
} else {
&#xA0; &#xA0; fwrite($fp, &quot;Data sent by socket&quot;);
&#xA0; &#xA0; $content = &quot;&quot;;
&#xA0; &#xA0; while (!feof($fp)) {&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $content .= fread($fp, 1024);
&#xA0; &#xA0; &#xA0; &#xA0; $stream_meta_data = stream_get_meta_data($fp); //Added line
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if($stream_meta_data[&apos;unread_bytes&apos;] &lt;= 0) break; //Added line
&#xA0; &#xA0; }
&#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; echo $content;
}
?>
```


Hope this will save a lot of headaches to someone.

(Greetings, from La Paz-Bolivia)

  

#



For anyone still trying to write an effective file downloader function/script, the work has been done for you in all the major servers including Apache &amp; nginx.

Using the X-Sendfile header, you can do the following:

if ($user-&gt;isLoggedIn())
{
&#xA0; &#xA0; header(&quot;X-Sendfile: $path_to_somefile_private&quot;);
&#xA0; &#xA0; header(&quot;Content-Type: application/octet-stream&quot;);
&#xA0; &#xA0; header(&quot;Content-Disposition: attachment; filename=\&quot;$somefile\&quot;&quot;);
}

Apache will serve the file for you while NOT revealing your private file path! Pretty nice. This works on all browsers/download managers and saves a lot of resources.

Documentation:
Apache module: https://tn123.org/mod_xsendfile/
Nginx: http://wiki.nginx.org/XSendfile
Lighttpd: http://blog.lighttpd.net/articles/2006/07/02/x-sendfile/

Hopefully this will save you many hours of work.

  

#

[Official documentation page](https://www.php.net/manual/en/function.fread.php)

**[To root](/README.md)**