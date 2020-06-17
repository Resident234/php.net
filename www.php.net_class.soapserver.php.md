# The SoapServer class




<div class="phpcode"><span class="html">
While there are plenty of mentions online that SoapServer doesn&apos;t support SOAP Headers, this isn&apos;t true.
<br>
<br>In your class, if you declare a function with the name of the header, the function will be called when that header is received.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">MySoapService </span><span class="keyword">{
<br>&#xA0; private </span><span class="default">$user_is_valid</span><span class="keyword">;
<br>
<br>&#xA0; function </span><span class="default">MyHeader</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">) {
<br>&#xA0; &#xA0; if ((isset(</span><span class="default">$header</span><span class="keyword">-&gt;</span><span class="default">Username</span><span class="keyword">)) &amp;&amp; (isset(</span><span class="default">$header</span><span class="keyword">-&gt;</span><span class="default">Password</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">ValidateUser</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">-&gt;</span><span class="default">Username</span><span class="keyword">, </span><span class="default">$header</span><span class="keyword">-&gt;</span><span class="default">Password</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_is_valid </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; }
<br>
<br>&#xA0; function </span><span class="default">MySoapRequest</span><span class="keyword">(</span><span class="default">$request</span><span class="keyword">) {
<br>&#xA0; &#xA0; if (</span><span class="default">$user_is_valid</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// process request
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; throw new </span><span class="default">MyFault</span><span class="keyword">(</span><span class="string">&quot;MySoapRequest&quot;</span><span class="keyword">, </span><span class="string">&quot;User not valid.&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapserver.php)

**[To root](/README.md)**