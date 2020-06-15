# curl_multi_exec




<div class="phpcode"><span class="html">
Solve CPU 100% usage, a more simple and right way:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">do {<br>&#xA0; &#xA0; </span><span class="default">curl_multi_exec</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">, </span><span class="default">$running</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">curl_multi_select</span><span class="keyword">(</span><span class="default">$mh</span><span class="keyword">);<br>} while (</span><span class="default">$running </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Probably you also want to be able to download the HTML content into buffers/variables, for parsing the HTML or for other processing in your program.<br><br>The example code on this page only outputs everything on the screen, without giving you the possibility to save the downloaded pages in string variables. Because downloading multiple pages is what I wanted to do (not a big surprise, huh? that&apos;s the reason for using multi-page parallel Curl) I was initially baffled, because this page doesn&apos;t give pointers to a guide how to do that.<br><br>Fortunately, there&apos;s a way to download content with parallel Curl requests (just like you would do for a single download with the regular curl_exec). You need to use: <a href="http://php.net/manual/en/function.curl-multi-getcontent.php" rel="nofollow" target="_blank">http://php.net/manual/en/function.curl-multi-getcontent.php</a><br><br>The function curl_multi_getcontent should definitely be mentioned in the &quot;See Also&quot; section of curl_multi_exec. Probably most people who find their way to the docs page of curl_multi_exec, actually want to download the multiple HTML pages (or other content from the multiple parallel Curl connections) into buffers, one page per one buffer.</span>
</div>
  

#


<div class="phpcode"><span class="html">
// Todas url gravadas em array<br>$url[] = &apos;<a href="http://www.link1.com.br" rel="nofollow" target="_blank">http://www.link1.com.br</a>&apos;;<br>$url[] = &apos;<a href="https://www.link2.com.br" rel="nofollow" target="_blank">https://www.link2.com.br</a>&apos;;<br>$url[] = &apos;<a href="https://www.link3.com.br" rel="nofollow" target="_blank">https://www.link3.com.br</a>&apos;;<br><br>// Setando op&#xE7;&#xE3;o padr&#xE3;o para todas url e adicionando a fila para processamento<br>$mh = curl_multi_init();<br>foreach($url as $key =&gt; $value){<br>&#xA0; $ch[$key] = curl_init($value);<br>&#xA0; curl_setopt($ch[$key], CURLOPT_NOBODY, true);<br>&#xA0; curl_setopt($ch[$key], CURLOPT_HEADER, true);<br>&#xA0; curl_setopt($ch[$key], CURLOPT_RETURNTRANSFER, true);<br>&#xA0; curl_setopt($ch[$key], CURLOPT_SSL_VERIFYPEER, false);<br>&#xA0; curl_setopt($ch[$key], CURLOPT_SSL_VERIFYHOST, false);<br>&#xA0; <br>&#xA0; curl_multi_add_handle($mh,$ch[$key]);<br>}<br><br>// Executando consulta<br>do {<br>&#xA0; curl_multi_exec($mh, $running);<br>&#xA0; curl_multi_select($mh);<br>} while ($running &gt; 0);<br><br>// Obtendo dados de todas as consultas e retirando da fila<br>foreach(array_keys($ch) as $key){<br>&#xA0; echo curl_getinfo($ch[$key], CURLINFO_HTTP_CODE);<br>&#xA0; echo curl_getinfo($ch[$key], CURLINFO_EFFECTIVE_URL);<br>&#xA0; echo &quot;\n&quot;;<br>&#xA0; <br>&#xA0; curl_multi_remove_handle($mh, $ch[$key]);<br>}<br><br>// Finalizando<br>curl_multi_close($mh);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-exec.php)

**[To root](/README.md)**