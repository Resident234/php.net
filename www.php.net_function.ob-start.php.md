# ob_start




<div class="phpcode"><span class="html">
The following should be added: &quot;If outbut buffering is still active when the script ends, PHP outputs it automatically. In effect, every script ends with ob_end_flush().&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use PHP to generate a static HTML page.&#xA0; Useful if you have a complex script that, for performance reasons, you do not want site visitors to run repeatedly on demand.&#xA0; A &quot;cron&quot; job can execute the PHP script to create the HTML page.&#xA0; For example:<br><br><span class="default">&lt;?php </span><span class="comment">// CREATE index.html<br>&#xA0;&#xA0; </span><span class="default">ob_start</span><span class="keyword">();<br></span><span class="comment">/* PERFORM COMLEX QUERY, ECHO RESULTS, ETC. */<br>&#xA0;&#xA0; </span><span class="default">$page </span><span class="keyword">= </span><span class="default">ob_get_contents</span><span class="keyword">();<br>&#xA0;&#xA0; </span><span class="default">ob_end_clean</span><span class="keyword">();<br>&#xA0;&#xA0; </span><span class="default">$cwd </span><span class="keyword">= </span><span class="default">getcwd</span><span class="keyword">();<br>&#xA0;&#xA0; </span><span class="default">$file </span><span class="keyword">= </span><span class="string">&quot;</span><span class="default">$cwd</span><span class="string">&quot; </span><span class="keyword">.</span><span class="string">&apos;/&apos;</span><span class="keyword">. </span><span class="string">&quot;index.html&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; @</span><span class="default">chmod</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">,</span><span class="default">0755</span><span class="keyword">);<br>&#xA0;&#xA0; </span><span class="default">$fw </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="string">&quot;w&quot;</span><span class="keyword">);<br>&#xA0;&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fw</span><span class="keyword">,</span><span class="default">$page</span><span class="keyword">, </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$page</span><span class="keyword">));<br>&#xA0;&#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fw</span><span class="keyword">);<br>&#xA0;&#xA0; die();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re using object-orientated code in PHP you may, like me, want to use a call-back function that is inside an object (i.e. a class function). In this case you send ob_start a two-element array as its single argument. The first element is the name of the object (without the $ at the start), and the second is the function to call. So to use a function &apos;indent&apos; in an object called &apos;$template&apos; you would use <span class="default">&lt;?php ob_start</span><span class="keyword">(array(</span><span class="string">&apos;template&apos;</span><span class="keyword">, </span><span class="string">&apos;indent&apos;</span><span class="keyword">)); </span><span class="default">?&gt;</span>.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-start.php)

**[⬆ to root](/)**