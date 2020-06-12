# headers_list




<div class="phpcode"><span class="html">
This function won&apos;t work for when you&apos;re running PHP from the command line. If will always return an empty array. This can be an issue when testing your project using PHPUnit or Codeception.<br><br>To solve this, install the xdebug extension and use `xdebug_get_headers` when on the cli.<br><br><span class="default">&lt;?php<br>$headers </span><span class="keyword">= </span><span class="default">php_sapi_name</span><span class="keyword">() === </span><span class="string">&apos;cli&apos; </span><span class="keyword">? </span><span class="default">xdebug_get_headers</span><span class="keyword">() : </span><span class="default">headers_list</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
note that it does not return the status header<br><br><span class="default">&lt;?php<br><br>header</span><span class="keyword">(</span><span class="string">&apos;HTTP/1.1 301 Moved Permanently&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">301</span><span class="keyword">);<br><br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;foo: bar&apos;</span><span class="keyword">);<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;a: b&apos;</span><span class="keyword">);<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;colon less example&apos;</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">headers_list</span><span class="keyword">());<br></span><span class="default">?&gt;<br></span><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; X-Powered-By: PHP/5.4.7<br>&#xA0; &#xA0; [1] =&gt; foo: bar<br>&#xA0; &#xA0; [2] =&gt; a: b<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.headers-list.php)

**[⬆ to root](/)**