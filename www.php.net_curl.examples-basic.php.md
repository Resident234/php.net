# Basic curl example




<div class="phpcode"><span class="html">
IMO this example would have been better if it had done a check for curl_error(), in order to advertize the existence of this function to people learning about cURL who try the example but mysteriously get no response back for whatever reason.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is important to notice that when using curl to post form data and you use an array for CURLOPT_POSTFIELDS option, the post will be in multipart format<br><br><span class="default">&lt;?php<br>$params</span><span class="keyword">=[</span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;John&apos;</span><span class="keyword">, </span><span class="string">&apos;surname&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;Doe&apos;</span><span class="keyword">, </span><span class="string">&apos;age&apos;</span><span class="keyword">=&gt;</span><span class="default">36</span><span class="keyword">)<br></span><span class="default">$defaults </span><span class="keyword">= array(<br></span><span class="default">CURLOPT_URL </span><span class="keyword">=&gt; </span><span class="string">&apos;<a href="http://myremoteservice/" rel="nofollow" target="_blank">http://myremoteservice/</a>&apos;</span><span class="keyword">, <br></span><span class="default">CURLOPT_POST </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,<br></span><span class="default">CURLOPT_POSTFIELDS </span><span class="keyword">=&gt; </span><span class="default">$params</span><span class="keyword">,<br>);<br></span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();<br></span><span class="default">curl_setopt_array</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, (</span><span class="default">$options </span><span class="keyword">+ </span><span class="default">$defaults</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span>This produce the following post header:<br><br>--------------------------fd1c4191862e3566<br>Content-Disposition: form-data; name=&quot;name&quot;<br><br>Jhon<br>--------------------------fd1c4191862e3566<br>Content-Disposition: form-data; name=&quot;surnname&quot;<br><br>Doe<br>--------------------------fd1c4191862e3566<br>Content-Disposition: form-data; name=&quot;age&quot;<br><br>36<br>--------------------------fd1c4191862e3566--<br><br>Setting CURLOPT_POSTFIELDS as follow produce a standard post header<br><br>CURLOPT_POSTFIELDS =&gt; http_build_query($params),<br><br>Which is:<br>name=John&amp;surname=Doe&amp;age=36<br><br>This caused me 2 days of debug while interacting with a java service which was sensible to this difference, while the equivalent one in php got both format without problem.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/curl.examples-basic.php)

**[⬆ to root](/)**