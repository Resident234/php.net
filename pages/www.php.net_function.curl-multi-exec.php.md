# curl_multi_exec



Solve CPU 100% usage, a more simple and right way:<br><br>

```
<?php

do {
    curl_multi_exec($mh, $running);
    curl_multi_select($mh);
} while ($running > 0);

?>
```
  

---

Probably you also want to be able to download the HTML content into buffers/variables, for parsing the HTML or for other processing in your program.<br><br>The example code on this page only outputs everything on the screen, without giving you the possibility to save the downloaded pages in string variables. Because downloading multiple pages is what I wanted to do (not a big surprise, huh? that&apos;s the reason for using multi-page parallel Curl) I was initially baffled, because this page doesn&apos;t give pointers to a guide how to do that.<br><br>Fortunately, there&apos;s a way to download content with parallel Curl requests (just like you would do for a single download with the regular curl_exec). You need to use: http://php.net/manual/en/function.curl-multi-getcontent.php<br><br>The function curl_multi_getcontent should definitely be mentioned in the "See Also" section of curl_multi_exec. Probably most people who find their way to the docs page of curl_multi_exec, actually want to download the multiple HTML pages (or other content from the multiple parallel Curl connections) into buffers, one page per one buffer.  

---

// Todas url gravadas em array<br>$url[] = &apos;http://www.link1.com.br&apos;;<br>$url[] = &apos;https://www.link2.com.br&apos;;<br>$url[] = &apos;https://www.link3.com.br&apos;;<br><br>// Setando op&#xE7;&#xE3;o padr&#xE3;o para todas url e adicionando a fila para processamento<br>$mh = curl_multi_init();<br>foreach($url as $key =&gt; $value){<br>  $ch[$key] = curl_init($value);<br>  curl_setopt($ch[$key], CURLOPT_NOBODY, true);<br>  curl_setopt($ch[$key], CURLOPT_HEADER, true);<br>  curl_setopt($ch[$key], CURLOPT_RETURNTRANSFER, true);<br>  curl_setopt($ch[$key], CURLOPT_SSL_VERIFYPEER, false);<br>  curl_setopt($ch[$key], CURLOPT_SSL_VERIFYHOST, false);<br>  <br>  curl_multi_add_handle($mh,$ch[$key]);<br>}<br><br>// Executando consulta<br>do {<br>  curl_multi_exec($mh, $running);<br>  curl_multi_select($mh);<br>} while ($running &gt; 0);<br><br>// Obtendo dados de todas as consultas e retirando da fila<br>foreach(array_keys($ch) as $key){<br>  echo curl_getinfo($ch[$key], CURLINFO_HTTP_CODE);<br>  echo curl_getinfo($ch[$key], CURLINFO_EFFECTIVE_URL);<br>  echo "\n";<br>  <br>  curl_multi_remove_handle($mh, $ch[$key]);<br>}<br><br>// Finalizando<br>curl_multi_close($mh);  

---

[Official documentation page](https://www.php.net/manual/en/function.curl-multi-exec.php)

**[To root](/README.md)**