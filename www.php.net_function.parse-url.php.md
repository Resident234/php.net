# parse_url




<div class="phpcode"><span class="html">
[If you haven&apos;t yet] been able to find a simple conversion back to string from a parsed url, here&apos;s an example:
<br>
<br><span class="default">&lt;?php
<br>
<br>$url </span><span class="keyword">= </span><span class="string">&apos;<a href="http://usr:pss@example.com:81/mypath/myfile.html?a=b&amp;b[]=2&amp;b[]=3#myfragment" rel="nofollow" target="_blank">http://usr:pss@example.com:81/mypath/myfile.html?a=b&amp;b[]=2&amp;b[]=3#myfragment</a>&apos;</span><span class="keyword">;
<br>if (</span><span class="default">$url </span><span class="keyword">=== </span><span class="default">unparse_url</span><span class="keyword">(</span><span class="default">parse_url</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">))) {
<br>&#xA0; print </span><span class="string">&quot;YES, they match!\n&quot;</span><span class="keyword">;
<br>}
<br>
<br>function </span><span class="default">unparse_url</span><span class="keyword">(</span><span class="default">$parsed_url</span><span class="keyword">) {
<br>&#xA0; </span><span class="default">$scheme&#xA0;&#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;scheme&apos;</span><span class="keyword">]) ? </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;scheme&apos;</span><span class="keyword">] . </span><span class="string">&apos;://&apos; </span><span class="keyword">: </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$host&#xA0; &#xA0;&#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;host&apos;</span><span class="keyword">]) ? </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;host&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$port&#xA0; &#xA0;&#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;port&apos;</span><span class="keyword">]) ? </span><span class="string">&apos;:&apos; </span><span class="keyword">. </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;port&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$user&#xA0; &#xA0;&#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;user&apos;</span><span class="keyword">]) ? </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;user&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$pass&#xA0; &#xA0;&#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;pass&apos;</span><span class="keyword">]) ? </span><span class="string">&apos;:&apos; </span><span class="keyword">. </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;pass&apos;</span><span class="keyword">]&#xA0; : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$pass&#xA0; &#xA0;&#xA0; </span><span class="keyword">= (</span><span class="default">$user </span><span class="keyword">|| </span><span class="default">$pass</span><span class="keyword">) ? </span><span class="string">&quot;</span><span class="default">$pass</span><span class="string">@&quot; </span><span class="keyword">: </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$path&#xA0; &#xA0;&#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;path&apos;</span><span class="keyword">]) ? </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;path&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$query&#xA0; &#xA0; </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;query&apos;</span><span class="keyword">]) ? </span><span class="string">&apos;?&apos; </span><span class="keyword">. </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;query&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$fragment </span><span class="keyword">= isset(</span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;fragment&apos;</span><span class="keyword">]) ? </span><span class="string">&apos;#&apos; </span><span class="keyword">. </span><span class="default">$parsed_url</span><span class="keyword">[</span><span class="string">&apos;fragment&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; return </span><span class="string">&quot;</span><span class="default">$scheme$user$pass$host$port$path$query$fragment</span><span class="string">&quot;</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is utf-8 compatible parse_url() replacement function based on &quot;laszlo dot janszky at gmail dot com&quot; work. Original incorrectly handled URLs with user:pass. Also made PHP 5.5 compatible (got rid of now deprecated regex /e modifier).<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * UTF-8 aware parse_url() replacement.<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * @return array<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">mb_parse_url</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$enc_url </span><span class="keyword">= </span><span class="default">preg_replace_callback</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;%[^:/@?&amp;=#]+%usD&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; function (</span><span class="default">$matches</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">urlencode</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; },<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$url<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$parts </span><span class="keyword">= </span><span class="default">parse_url</span><span class="keyword">(</span><span class="default">$enc_url</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$parts </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \</span><span class="default">InvalidArgumentException</span><span class="keyword">(</span><span class="string">&apos;Malformed URL: &apos; </span><span class="keyword">. </span><span class="default">$url</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$parts </span><span class="keyword">as </span><span class="default">$name </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$parts</span><span class="keyword">[</span><span class="default">$name</span><span class="keyword">] = </span><span class="default">urldecode</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$parts</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It may be worth reminding that the value of the #fragment never gets sent to the server.&#xA0; Anchors processing is exclusively client-side.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I was writing unit tests and needed to cause this function to kick out an error and return FALSE in order to test a specific execution path. If anyone else needs to force a failure, the following inputs will work:
<br>
<br><span class="default">&lt;?php
<br>parse_url</span><span class="keyword">(</span><span class="string">&quot;<a href="http:///example.com" rel="nofollow" target="_blank">http:///example.com</a>&quot;</span><span class="keyword">);
<br></span><span class="default">parse_url</span><span class="keyword">(</span><span class="string">&quot;<a href="http://:80" rel="nofollow" target="_blank">http://:80</a>&quot;</span><span class="keyword">);
<br></span><span class="default">parse_url</span><span class="keyword">(</span><span class="string">&quot;<a href="http://user@:80" rel="nofollow" target="_blank">http://user@:80</a>&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a good way to using parse_url () gets the youtube link.<br>This function I used in many works:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">youtube</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="default">$width</span><span class="keyword">=</span><span class="default">560</span><span class="keyword">, </span><span class="default">$height</span><span class="keyword">=</span><span class="default">315</span><span class="keyword">, </span><span class="default">$fullscreen</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">parse_str</span><span class="keyword">( </span><span class="default">parse_url</span><span class="keyword">( </span><span class="default">$url</span><span class="keyword">, </span><span class="default">PHP_URL_QUERY </span><span class="keyword">), </span><span class="default">$my_array_of_vars </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$youtube</span><span class="keyword">= </span><span class="string">&apos;&lt;iframe allowtransparency=&quot;true&quot; scrolling=&quot;no&quot; width=&quot;&apos;</span><span class="keyword">.</span><span class="default">$width</span><span class="keyword">.</span><span class="string">&apos;&quot; height=&quot;&apos;</span><span class="keyword">.</span><span class="default">$height</span><span class="keyword">.</span><span class="string">&apos;&quot; src=&quot;//www.youtube.com/embed/&apos;</span><span class="keyword">.</span><span class="default">$my_array_of_vars</span><span class="keyword">[</span><span class="string">&apos;v&apos;</span><span class="keyword">].</span><span class="string">&apos;&quot; frameborder=&quot;0&quot;&apos;</span><span class="keyword">.(</span><span class="default">$fullscreen</span><span class="keyword">?</span><span class="string">&apos; allowfullscreen&apos;</span><span class="keyword">:</span><span class="default">NULL</span><span class="keyword">).</span><span class="string">&apos;&gt;&lt;/iframe&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="default">$youtube</span><span class="keyword">;<br>}<br><br></span><span class="comment">// show youtube on my page<br></span><span class="default">$url</span><span class="keyword">=</span><span class="string">&apos;<a href="http://www.youtube.com/watch?v=yvTd6XxgCBE" rel="nofollow" target="_blank">http://www.youtube.com/watch?v=yvTd6XxgCBE</a>&apos;</span><span class="keyword">;<br> </span><span class="default">youtube</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="default">560</span><span class="keyword">, </span><span class="default">315</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>parse_url () allocates a unique youtube code and&#xA0; put into iframe link and displayed on your page. The size of the videos choose yourself.<br><br>Enjoy.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-url.php)

**[⬆ to root](/)**