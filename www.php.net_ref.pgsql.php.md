# PostgreSQL Functions




<div class="phpcode"><span class="html">
A simple conversion for 1D PostgreSQL array data:<br><br>// =====<br>//Example #1 (An array of IP addresses):<br><span class="default">&lt;?php<br>&#xA0; $pgsqlArr </span><span class="keyword">= </span><span class="string">&apos;{192.168.1.1,10.1.1.1}&apos;</span><span class="keyword">;<br><br>&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/^{(.*)}$/&apos;</span><span class="keyword">, </span><span class="default">$pgsqlArr</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);<br>&#xA0; </span><span class="default">$phpArr </span><span class="keyword">= </span><span class="default">str_getcsv</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]);<br><br>&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$phpArr</span><span class="keyword">);<br>}<br></span><span class="comment">// Output:<br>// Array<br>// (<br>//&#xA0; &#xA0; [0] =&gt; 192.168.1.1<br>//&#xA0; &#xA0; [1] =&gt; 10.1.1.1<br>// )<br>// =====<br><br>// =====<br>// Example #2 (An array of strings including spaces and commas):<br></span><span class="keyword">&lt;?</span><span class="default">php<br>&#xA0; $pgsqlArr </span><span class="keyword">= </span><span class="string">&apos;{string1,string2,&quot;string,3&quot;,&quot;string 4&quot;}&apos;</span><span class="keyword">;<br><br>&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/^{(.*)}$/&apos;</span><span class="keyword">, </span><span class="default">$pgsqlArr</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);<br>&#xA0; </span><span class="default">$phpArr </span><span class="keyword">= </span><span class="default">str_getcsv</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]);<br><br>&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$phpArr</span><span class="keyword">);<br>}<br></span><span class="comment">// Output:<br>// Array<br>// (<br>//&#xA0; &#xA0; [0] =&gt; string1<br>//&#xA0; &#xA0; [1] =&gt; string2<br>//&#xA0; &#xA0; [2] =&gt; string,3<br>//&#xA0; &#xA0; [3] =&gt; string 4<br>// )<br>// =====</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.pgsql.php)

**[⬆ to root](/)**