# The SoapClient class




<div class="phpcode"><span class="html">
When you need to connect to services requiring to send extra header use this method.
<br>
<br>Here how we can to it with PHP and SoapClient
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">exampleChannelAdvisorAuth
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; public </span><span class="default">$DeveloperKey</span><span class="keyword">;
<br>&#xA0; &#xA0; public </span><span class="default">$Password</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$pass</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">DeveloperKey </span><span class="keyword">= </span><span class="default">$key</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">Password </span><span class="keyword">= </span><span class="default">$pass</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$devKey&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br></span><span class="default">$password&#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br></span><span class="default">$accountId&#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment">// Create the SoapClient instance
<br></span><span class="default">$url&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br></span><span class="default">$client&#xA0; &#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, array(</span><span class="string">&quot;trace&quot; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&quot;exception&quot; </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">));
<br>
<br></span><span class="comment">// Create the header
<br></span><span class="default">$auth&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">ChannelAdvisorAuth</span><span class="keyword">(</span><span class="default">$devKey</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">);
<br></span><span class="default">$header&#xA0; &#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">SoapHeader</span><span class="keyword">(</span><span class="string">&quot;<a href="http://www.example.com/webservices/" rel="nofollow" target="_blank">http://www.example.com/webservices/</a>&quot;</span><span class="keyword">, </span><span class="string">&quot;APICredentials&quot;</span><span class="keyword">, </span><span class="default">$auth</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br>
<br></span><span class="comment">// Call wsdl function
<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">__soapCall</span><span class="keyword">(</span><span class="string">&quot;DeleteMarketplaceAd&quot;</span><span class="keyword">, array(
<br>&#xA0; &#xA0; </span><span class="string">&quot;DeleteMarketplaceAd&quot; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;accountID&quot;&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">$accountId</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;marketplaceAdID&quot;&#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;9938745&quot;&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// The ads ID
<br>&#xA0; &#xA0; </span><span class="keyword">)
<br>), </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$header</span><span class="keyword">);
<br>
<br></span><span class="comment">// Echo the result
<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;pre&gt;&quot;</span><span class="keyword">.</span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">).</span><span class="string">&quot;&lt;/pre&gt;&quot;</span><span class="keyword">;
<br>if(</span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">DeleteMarketplaceAdResult</span><span class="keyword">-&gt;</span><span class="default">Status </span><span class="keyword">== </span><span class="string">&quot;Success&quot;</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Item deleted!&quot;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapclient.php)

**[To root](/README.md)**