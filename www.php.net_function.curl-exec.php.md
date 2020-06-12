# curl_exec




<div class="phpcode"><span class="html">
Just in case anyone is looking for a a couple of simple functions [to help automate cURL processes for POST and GET queries] I thought I&apos;d post these.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">/**
<br> * Send a POST requst using cURL
<br> * @param string $url to request
<br> * @param array $post values to send
<br> * @param array $options for cURL
<br> * @return string
<br> */
<br></span><span class="keyword">function </span><span class="default">curl_post</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, array </span><span class="default">$post </span><span class="keyword">= </span><span class="default">NULL</span><span class="keyword">, array </span><span class="default">$options </span><span class="keyword">= array())
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$defaults </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_POST </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_HEADER </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_URL </span><span class="keyword">=&gt; </span><span class="default">$url</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_FRESH_CONNECT </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_RETURNTRANSFER </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_FORBID_REUSE </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_TIMEOUT </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_POSTFIELDS </span><span class="keyword">=&gt; </span><span class="default">http_build_query</span><span class="keyword">(</span><span class="default">$post</span><span class="keyword">)
<br>&#xA0; &#xA0; );
<br>
<br>&#xA0; &#xA0; </span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();
<br>&#xA0; &#xA0; </span><span class="default">curl_setopt_array</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, (</span><span class="default">$options </span><span class="keyword">+ </span><span class="default">$defaults</span><span class="keyword">));
<br>&#xA0; &#xA0; if( ! </span><span class="default">$result </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="default">curl_error</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br>
<br></span><span class="comment">/**
<br> * Send a GET requst using cURL
<br> * @param string $url to request
<br> * @param array $get values to send
<br> * @param array $options for cURL
<br> * @return string
<br> */
<br></span><span class="keyword">function </span><span class="default">curl_get</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, array </span><span class="default">$get </span><span class="keyword">= </span><span class="default">NULL</span><span class="keyword">, array </span><span class="default">$options </span><span class="keyword">= array())
<br>{&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$defaults </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_URL </span><span class="keyword">=&gt; </span><span class="default">$url</span><span class="keyword">. (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="string">&apos;?&apos;</span><span class="keyword">) === </span><span class="default">FALSE </span><span class="keyword">? </span><span class="string">&apos;?&apos; </span><span class="keyword">: </span><span class="string">&apos;&apos;</span><span class="keyword">). </span><span class="default">http_build_query</span><span class="keyword">(</span><span class="default">$get</span><span class="keyword">),
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_HEADER </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_RETURNTRANSFER </span><span class="keyword">=&gt; </span><span class="default">TRUE</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_TIMEOUT </span><span class="keyword">=&gt; </span><span class="default">4
<br>&#xA0; &#xA0; </span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();
<br>&#xA0; &#xA0; </span><span class="default">curl_setopt_array</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, (</span><span class="default">$options </span><span class="keyword">+ </span><span class="default">$defaults</span><span class="keyword">));
<br>&#xA0; &#xA0; if( ! </span><span class="default">$result </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="default">curl_error</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Don&apos;t disable SSL verification! You don&apos;t need to, and it&apos;s super easy to stay secure! If you found that turning off &quot;CURLOPT_SSL_VERIFYHOST&quot; and &quot;CURLOPT_SSL_VERIFYPEER&quot; solved your problem, odds are you&apos;re just on a Windows box. Takes 2 min to solve the problem. Walkthrough here:<br><br><a href="https://snippets.webaware.com.au/howto/stop-turning-off-curlopt_ssl_verifypeer-and-fix-your-php-config/" rel="nofollow" target="_blank">https://snippets.webaware.com.au/howto/stop-turning-off-curlopt_ssl_verifypeer-and-fix-your-php-config/</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful when using curl_exec() and the CURLOPT_RETURNTRANSFER option. According to the manual and assorted documentation:<br>Set CURLOPT_RETURNTRANSFER to TRUE to return the transfer as a string of the return value of curl_exec() instead of outputting it out directly.<br><br>When retrieving a document with no content (ie. 0 byte file), curl_exec() will return bool(true), not an empty string. I&apos;ve not seen any mention of this in the manual.<br><br>Example code to reproduce this:<br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="comment">// fictional URL to an existing file with no data in it (ie. 0 byte file)<br>&#xA0; &#xA0; </span><span class="default">$url </span><span class="keyword">= </span><span class="string">&apos;<a href="http://www.example.com/empty_file.txt" rel="nofollow" target="_blank">http://www.example.com/empty_file.txt</a>&apos;</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="default">$curl </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$url</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_HEADER</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// execute and return string (this should be an empty string &apos;&apos;)<br>&#xA0; &#xA0; </span><span class="default">$str </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// the value of $str is actually bool(true), not empty string &apos;&apos;<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-exec.php)

**[⬆ to root](/)**