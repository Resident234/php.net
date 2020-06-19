# SoapClient::__soapCall




<div class="phpcode"><span class="html">
Note that calling __soapCall and calling the generated method from WSDL requires specifying parameters in two different ways.<br><br>For example, if you have a web service with method login that takes username and password, you can call it the following way:<br><span class="default">&lt;?php<br>$params </span><span class="keyword">= array(</span><span class="string">&apos;username&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;name&apos;</span><span class="keyword">, </span><span class="string">&apos;password&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;secret&apos;</span><span class="keyword">);<br></span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">login</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>If you want to call __soapCall, you must wrap the arguments in another array as follows:<br><span class="default">&lt;?php<br>$client</span><span class="keyword">-&gt;</span><span class="default">__soapCall</span><span class="keyword">(</span><span class="string">&apos;login&apos;</span><span class="keyword">, array(</span><span class="default">$params</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
One thing to note.<br><br>This happened to me and it took a while until I discovered what the problem was.<br><br>I was trying to get .NET objects from a provided web service, however it always seemed to return empty objects. It did return the backbone, but nothing within the objects that made up the structure.<br><br>Anyhow, it seems that you have to be very precise with the arrays when calling these functions. Par example, do this:<br><br><span class="default">&lt;?php<br>$obj </span><span class="keyword">= </span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">__soapCall</span><span class="keyword">(</span><span class="default">$SOAPCall</span><span class="keyword">, array(</span><span class="string">&apos;parameters&apos;</span><span class="keyword">=&gt;</span><span class="default">$SoapCallParameters</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>meaning that you must put an array as the second argument with &apos;parameters&apos; as the key and the soap call parameters as the value.<br><br>Also make sure that the parameter variable, in my case $SoapCallParameters is in the form of what is requested by the webservice.<br><br>So, don&apos;t just make an array of the form:<br><span class="default">&lt;?php<br><br></span><span class="keyword">(<br>&#xA0;&#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="string">&apos;Mary&apos;</span><span class="keyword">,<br>&#xA0;&#xA0; [</span><span class="default">1</span><span class="keyword">] =&gt; </span><span class="default">1983<br></span><span class="keyword">)<br><br></span><span class="default">?&gt;<br></span><br>but if the webservice requests a &apos;muid&apos; variable as &apos;Mary&apos; and a &apos;birthyear&apos; as 1983, then make your array like this:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">(<br>&#xA0;&#xA0; [</span><span class="default">muid</span><span class="keyword">] =&gt; </span><span class="string">&apos;Mary&apos;</span><span class="keyword">,<br>&#xA0;&#xA0; [</span><span class="default">birthyear</span><span class="keyword">] =&gt; </span><span class="default">1983<br></span><span class="keyword">)<br><br></span><span class="default">?&gt;<br></span><br>The above arrays refer to the $SoapCallParameters variable.<br><br>Hope this helps somebody, not having to spend too much time figuring out the problems.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.soapcall.php)

**[To root](/README.md)**