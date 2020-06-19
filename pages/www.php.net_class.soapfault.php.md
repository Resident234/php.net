# The SoapFault class




<div class="phpcode"><span class="html">
You may use undocumented and invisible property $e-&gt;faultcode to access string version of $code. Because standard $e-&gt;getCode() does not work:
<br>
<br><span class="default">&lt;?php
<br>$e </span><span class="keyword">= new </span><span class="default">SoapFault</span><span class="keyword">(</span><span class="string">&quot;test&quot;</span><span class="keyword">, </span><span class="string">&quot;msg&quot;</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getCode</span><span class="keyword">()); </span><span class="comment">// prints &quot;0&quot;
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">faultcode</span><span class="keyword">); </span><span class="comment">// prints &quot;test&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>Also you may use namespaced fault codes:
<br>
<br><span class="default">&lt;?php
<br>$e </span><span class="keyword">= new </span><span class="default">SoapFault</span><span class="keyword">(array(</span><span class="string">&quot;namespace&quot;</span><span class="keyword">, </span><span class="string">&quot;test&quot;</span><span class="keyword">), </span><span class="string">&quot;msg&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>- see ext/soap/soap.php, PHP_METHOD(SoapFault, SoapFault). To access the namespace, use $e-&gt;faultcodens</span>
</div>
  

#


<div class="phpcode"><span class="html">
A bit more digging in ext/soap/soap.c and the set_soap_fault function reveals the other undocumented properties from the constructor:<br><br><span class="default">&lt;?php<br></span><span class="keyword">try {<br>&#xA0; &#xA0; throw new </span><span class="default">SoapFault</span><span class="keyword">(</span><span class="string">&apos;code&apos;</span><span class="keyword">, </span><span class="string">&apos;string&apos;</span><span class="keyword">, </span><span class="string">&apos;actor&apos;</span><span class="keyword">, </span><span class="string">&apos;detail&apos;</span><span class="keyword">, </span><span class="string">&apos;name&apos;</span><span class="keyword">, </span><span class="string">&apos;header&apos;</span><span class="keyword">);<br>} catch (</span><span class="default">Exception $ex</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$ex</span><span class="keyword">-&gt;</span><span class="default">faultcode</span><span class="keyword">, </span><span class="default">$ex</span><span class="keyword">-&gt;</span><span class="default">faultstring</span><span class="keyword">, </span><span class="default">$ex</span><span class="keyword">-&gt;</span><span class="default">faultactor</span><span class="keyword">, </span><span class="default">$ex</span><span class="keyword">-&gt;</span><span class="default">detail</span><span class="keyword">, </span><span class="default">$ex</span><span class="keyword">-&gt;</span><span class="default">_name</span><span class="keyword">, </span><span class="default">$ex</span><span class="keyword">-&gt;</span><span class="default">headerfault</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapfault.php)

**[To root](/README.md)**