# http_build_query




<div class="phpcode"><span class="html">
Params with null value do not present in result string.
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= array(</span><span class="string">&apos;test&apos; </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">, </span><span class="string">&apos;test2&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);
<br>echo </span><span class="default">http_build_query</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>will produce:
<br>
<br>test2=1</span>
</div>
  

#


<div class="phpcode"><span class="html">
Passing null to $arg_separator is the same as passing an empty string, which is probably not what you want. <br><br>If you need to change the enc_type, use this:<br><br>&#xA0; &#xA0; http_build_query($query, null, &apos;&amp;&apos;, PHP_QUERY_RFC3986);<br><br>Or possibly this:<br><br>&#xA0; &#xA0; http_build_query($query, null, ini_get(&apos;arg_separator.output&apos;), PHP_QUERY_RFC3986);<br><br>But not this:<br><br>&#xA0; &#xA0; // BAD CODE!<br>&#xA0; &#xA0; http_build_query($query, null, null, PHP_QUERY_RFC3986);</span>
</div>
  

#


<div class="phpcode"><span class="html">
As noted before, with php5.3 the separator is &amp;amp; on some servers it seems. Normally if posting to another php5.3 machine this will not be a problem.<br><br>But if you post to a tomcat java server or something else the &amp;amp; might not be handled properly.<br><br>To overcome this specify:<br><br>http_build_query($array, &apos;&apos;, &apos;&amp;&apos;);<br><br>and NOT<br><br>http_build_query($array); //gives &amp;amp; to some servers</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function makes like this<br><br>files[0]=1&amp;files[1]=2&amp;...<br><br>To do it like this:<br><br>files[]=1&amp;files[]=2&amp;...<br><br>Do this:<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $query = http_build_query($query);<br>&#xA0; &#xA0; &#xA0; &#xA0; $query = preg_replace(&apos;/%5B[0-9]+%5D/simU&apos;, &apos;%5B%5D&apos;, $query);</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you send boolean values it transform in integer :<br><br>$a = [teste1= true,teste2=false];<br>echo http_build_query($a)<br><br>//result will be teste1=1&amp;teste2=0</span>
</div>
  

#


<div class="phpcode"><span class="html">
Is it worth noting that if query_data is an associative array and a value is itself an empty array, or an array of nothing but empty array (or arrays containing only empty arrays etc.), the corresponding key will not appear in the resulting query string?<br>E.g.<br><br>$post_data = array(&apos;name&apos;=&gt;&apos;miller&apos;, &apos;address&apos;=&gt;array(&apos;address_lines&apos;=&gt;array()), &apos;age&apos;=&gt;23);<br>echo http_build_query($post_data);<br><br>will print<br>name=miller&amp;age=23</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.http-build-query.php)

**[⬆ to root](/)**