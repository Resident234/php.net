# gzdecode





To decode / uncompress the received HTTP POST data in PHP code, request data coming from Java / Android application via HTTP POST GZIP / DEFLATE compressed format

1) Data sent from Java Android app to PHP using DeflaterOutputStream java class and received in PHP as shown below
echo gzinflate( substr($HTTP_RAW_POST_DATA,2,-4) ) . PHP_EOL&#xA0; . PHP_EOL;

2) Data sent from Java Android app to PHP using GZIPOutputStream java class and received in PHP code as shown below
echo gzinflate( substr($HTTP_RAW_POST_DATA,10,-8) ) . PHP_EOL&#xA0; . PHP_EOL;

From Java Android side (API level 10+), data being sent in DEFLATE compressed format
&#xA0; &#xA0; &#xA0; &#xA0; String body = &quot;Lorem ipsum shizzle ma nizle&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; URL url = new URL(&quot;http://www.url.com/postthisdata.php&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; URLConnection conn = url.openConnection();
&#xA0; &#xA0; &#xA0; &#xA0; conn.setDoOutput(true);
&#xA0; &#xA0; &#xA0; &#xA0; conn.setRequestProperty(&quot;Content-encoding&quot;, &quot;deflate&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; conn.setRequestProperty(&quot;Content-type&quot;, &quot;application/octet-stream&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; DeflaterOutputStream dos = new DeflaterOutputStream(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; conn.getOutputStream());
&#xA0; &#xA0; &#xA0; &#xA0; dos.write(body.getBytes());
&#xA0; &#xA0; &#xA0; &#xA0; dos.flush();
&#xA0; &#xA0; &#xA0; &#xA0; dos.close();
&#xA0; &#xA0; &#xA0; &#xA0; BufferedReader in = new BufferedReader(new InputStreamReader(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; conn.getInputStream()));
&#xA0; &#xA0; &#xA0; &#xA0; String decodedString = &quot;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; while ((decodedString = in.readLine()) != null) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Log.e(&quot;dump&quot;,decodedString);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; in.close();

On PHP side (v 5.3.1), code for decompressing this DEFLATE data will be
&#xA0; &#xA0; echo substr($HTTP_RAW_POST_DATA,2,-4);

From Java Android side (API level 10+), data being sent in GZIP compressed format

&#xA0; &#xA0; &#xA0; &#xA0; String body1 = &quot;Lorem ipsum shizzle ma nizle&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; URL url1 = new URL(&quot;http://www.url.com/postthisdata.php&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; URLConnection conn1 = url1.openConnection();
&#xA0; &#xA0; &#xA0; &#xA0; conn1.setDoOutput(true);
&#xA0; &#xA0; &#xA0; &#xA0; conn1.setRequestProperty(&quot;Content-encoding&quot;, &quot;gzip&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; conn1.setRequestProperty(&quot;Content-type&quot;, &quot;application/octet-stream&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; GZIPOutputStream dos1 = new GZIPOutputStream(conn1.getOutputStream());
&#xA0; &#xA0; &#xA0; &#xA0; dos1.write(body1.getBytes());
&#xA0; &#xA0; &#xA0; &#xA0; dos1.flush();
&#xA0; &#xA0; &#xA0; &#xA0; dos1.close();
&#xA0; &#xA0; &#xA0; &#xA0; BufferedReader in1 = new BufferedReader(new InputStreamReader(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; conn1.getInputStream()));
&#xA0; &#xA0; &#xA0; &#xA0; String decodedString1 = &quot;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; while ((decodedString1 = in1.readLine()) != null) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Log.e(&quot;dump&quot;,decodedString1);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; in1.close();

On PHP side (v 5.3.1), code for decompressing this GZIP data will be
&#xA0; &#xA0; echo substr($HTTP_RAW_POST_DATA,10,-8);

Useful PHP code for printing out compressed data using all available formats.

$data = &quot;Lorem ipsum shizzle ma nizle&quot;;
echo &quot;\n\n\n&quot;;
for($i=-1;$i&lt;=9;$i++)
&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzcompress($data,$i))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;
echo &quot;\n\n\n&quot;;
for($i=-1;$i&lt;=9;$i++)
&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzdeflate($data,$i))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;
echo &quot;\n\n\n&quot;;
for($i=-1;$i&lt;=9;$i++)
&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzencode($data,$i,FORCE_GZIP))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;
echo &quot;\n\n\n&quot;;
for($i=-1;$i&lt;=9;$i++)
&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzencode($data,$i,FORCE_DEFLATE))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;
echo &quot;\n\n\n&quot;;

Hope this helps. Please ThumbsUp if this saved you a lot of effort and time.

  

#



I don&apos;t have any deep knowledge in compression algorithms and formats, so I have no idea if this works the same on all platforms. But by several experiments on my Linux box I found how to get gzdecoded data without temporary files. Here it is:





```
<?php

function gzdecode($data)

{

&#xA0;&#xA0; return gzinflate(substr($data,10,-8));

}

?>
```




That&apos;s it. Simply strip header and footer bytes and you get raw deflated data for inflation.

  

#

[Official documentation page](https://www.php.net/manual/en/function.gzdecode.php)

**[To root](/README.md)**