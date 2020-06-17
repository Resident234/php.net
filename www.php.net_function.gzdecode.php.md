# gzdecode




<div class="phpcode"><span class="html">
To decode / uncompress the received HTTP POST data in PHP code, request data coming from Java / Android application via HTTP POST GZIP / DEFLATE compressed format<br><br>1) Data sent from Java Android app to PHP using DeflaterOutputStream java class and received in PHP as shown below<br>echo gzinflate( substr($HTTP_RAW_POST_DATA,2,-4) ) . PHP_EOL&#xA0; . PHP_EOL;<br><br>2) Data sent from Java Android app to PHP using GZIPOutputStream java class and received in PHP code as shown below<br>echo gzinflate( substr($HTTP_RAW_POST_DATA,10,-8) ) . PHP_EOL&#xA0; . PHP_EOL;<br><br>From Java Android side (API level 10+), data being sent in DEFLATE compressed format<br>&#xA0; &#xA0; &#xA0; &#xA0; String body = &quot;Lorem ipsum shizzle ma nizle&quot;;<br>&#xA0; &#xA0; &#xA0; &#xA0; URL url = new URL(&quot;<a href="http://www.url.com/postthisdata.php" rel="nofollow" target="_blank">http://www.url.com/postthisdata.php</a>&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; URLConnection conn = url.openConnection();<br>&#xA0; &#xA0; &#xA0; &#xA0; conn.setDoOutput(true);<br>&#xA0; &#xA0; &#xA0; &#xA0; conn.setRequestProperty(&quot;Content-encoding&quot;, &quot;deflate&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; conn.setRequestProperty(&quot;Content-type&quot;, &quot;application/octet-stream&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; DeflaterOutputStream dos = new DeflaterOutputStream(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; conn.getOutputStream());<br>&#xA0; &#xA0; &#xA0; &#xA0; dos.write(body.getBytes());<br>&#xA0; &#xA0; &#xA0; &#xA0; dos.flush();<br>&#xA0; &#xA0; &#xA0; &#xA0; dos.close();<br>&#xA0; &#xA0; &#xA0; &#xA0; BufferedReader in = new BufferedReader(new InputStreamReader(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; conn.getInputStream()));<br>&#xA0; &#xA0; &#xA0; &#xA0; String decodedString = &quot;&quot;;<br>&#xA0; &#xA0; &#xA0; &#xA0; while ((decodedString = in.readLine()) != null) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Log.e(&quot;dump&quot;,decodedString);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; in.close();<br><br>On PHP side (v 5.3.1), code for decompressing this DEFLATE data will be<br>&#xA0; &#xA0; echo substr($HTTP_RAW_POST_DATA,2,-4);<br><br>From Java Android side (API level 10+), data being sent in GZIP compressed format<br><br>&#xA0; &#xA0; &#xA0; &#xA0; String body1 = &quot;Lorem ipsum shizzle ma nizle&quot;;<br>&#xA0; &#xA0; &#xA0; &#xA0; URL url1 = new URL(&quot;<a href="http://www.url.com/postthisdata.php" rel="nofollow" target="_blank">http://www.url.com/postthisdata.php</a>&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; URLConnection conn1 = url1.openConnection();<br>&#xA0; &#xA0; &#xA0; &#xA0; conn1.setDoOutput(true);<br>&#xA0; &#xA0; &#xA0; &#xA0; conn1.setRequestProperty(&quot;Content-encoding&quot;, &quot;gzip&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; conn1.setRequestProperty(&quot;Content-type&quot;, &quot;application/octet-stream&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; GZIPOutputStream dos1 = new GZIPOutputStream(conn1.getOutputStream());<br>&#xA0; &#xA0; &#xA0; &#xA0; dos1.write(body1.getBytes());<br>&#xA0; &#xA0; &#xA0; &#xA0; dos1.flush();<br>&#xA0; &#xA0; &#xA0; &#xA0; dos1.close();<br>&#xA0; &#xA0; &#xA0; &#xA0; BufferedReader in1 = new BufferedReader(new InputStreamReader(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; conn1.getInputStream()));<br>&#xA0; &#xA0; &#xA0; &#xA0; String decodedString1 = &quot;&quot;;<br>&#xA0; &#xA0; &#xA0; &#xA0; while ((decodedString1 = in1.readLine()) != null) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Log.e(&quot;dump&quot;,decodedString1);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; in1.close();<br><br>On PHP side (v 5.3.1), code for decompressing this GZIP data will be<br>&#xA0; &#xA0; echo substr($HTTP_RAW_POST_DATA,10,-8);<br><br>Useful PHP code for printing out compressed data using all available formats.<br><br>$data = &quot;Lorem ipsum shizzle ma nizle&quot;;<br>echo &quot;\n\n\n&quot;;<br>for($i=-1;$i&lt;=9;$i++)<br>&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzcompress($data,$i))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;<br>echo &quot;\n\n\n&quot;;<br>for($i=-1;$i&lt;=9;$i++)<br>&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzdeflate($data,$i))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;<br>echo &quot;\n\n\n&quot;;<br>for($i=-1;$i&lt;=9;$i++)<br>&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzencode($data,$i,FORCE_GZIP))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;<br>echo &quot;\n\n\n&quot;;<br>for($i=-1;$i&lt;=9;$i++)<br>&#xA0; &#xA0; echo chunk_split(strtoupper(bin2hex(gzencode($data,$i,FORCE_DEFLATE))),2,&quot; &quot;) . PHP_EOL&#xA0; . PHP_EOL;<br>echo &quot;\n\n\n&quot;;<br><br>Hope this helps. Please ThumbsUp if this saved you a lot of effort and time.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I don&apos;t have any deep knowledge in compression algorithms and formats, so I have no idea if this works the same on all platforms. But by several experiments on my Linux box I found how to get gzdecoded data without temporary files. Here it is:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">gzdecode</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">)
<br>{
<br>&#xA0;&#xA0; return </span><span class="default">gzinflate</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">,</span><span class="default">10</span><span class="keyword">,-</span><span class="default">8</span><span class="keyword">));
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>That&apos;s it. Simply strip header and footer bytes and you get raw deflated data for inflation.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gzdecode.php)

**[To root](/README.md)**