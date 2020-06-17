# Geo IP Location




<div class="phpcode"><span class="html">
With GeoIP2, the easiest way is to:<br><br>* Grab the latest GeoIP2 Lite Database(s): <a href="https://dev.maxmind.com/geoip/geoip2/geolite2/" rel="nofollow" target="_blank">https://dev.maxmind.com/geoip/geoip2/geolite2/</a><br>* Grab the latest geoip2.phar: <a href="https://github.com/maxmind/GeoIP2-php/releases" rel="nofollow" target="_blank">https://github.com/maxmind/GeoIP2-php/releases</a><br><br><span class="default">&lt;?php<br></span><span class="keyword">require_once(</span><span class="string">&quot;geoip2.phar&quot;</span><span class="keyword">);<br>use </span><span class="default">GeoIp2</span><span class="keyword">\</span><span class="default">Database</span><span class="keyword">\</span><span class="default">Reader</span><span class="keyword">;<br></span><span class="comment">// City DB<br></span><span class="default">$reader </span><span class="keyword">= new </span><span class="default">Reader</span><span class="keyword">(</span><span class="string">&apos;/path/to/GeoLite2-City.mmdb&apos;</span><span class="keyword">);<br></span><span class="default">$record </span><span class="keyword">= </span><span class="default">$reader</span><span class="keyword">-&gt;</span><span class="default">city</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REMOTE_ADDR&apos;</span><span class="keyword">]);<br></span><span class="comment">// or for Country DB<br>// $reader = new Reader(&apos;/path/to/GeoLite2-Country.mmdb&apos;);<br>// $record = $reader-&gt;country($_SERVER[&apos;REMOTE_ADDR&apos;]);<br></span><span class="keyword">print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">country</span><span class="keyword">-&gt;</span><span class="default">isoCode </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">country</span><span class="keyword">-&gt;</span><span class="default">name </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">country</span><span class="keyword">-&gt;</span><span class="default">names</span><span class="keyword">[</span><span class="string">&apos;zh-CN&apos;</span><span class="keyword">] . </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">mostSpecificSubdivision</span><span class="keyword">-&gt;</span><span class="default">name </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">mostSpecificSubdivision</span><span class="keyword">-&gt;</span><span class="default">isoCode </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">city</span><span class="keyword">-&gt;</span><span class="default">name </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">postal</span><span class="keyword">-&gt;</span><span class="default">code </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">location</span><span class="keyword">-&gt;</span><span class="default">latitude </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>print(</span><span class="default">$record</span><span class="keyword">-&gt;</span><span class="default">location</span><span class="keyword">-&gt;</span><span class="default">longitude </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>$&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be noted that this extension has now been superseded by the GeoIP2 API that MaxMind now produces. There is a pure-PHP set of classes and a C library and extension you can optionally install. The code can be found in various projects on MaxMind&apos;s GitHub page: <a href="https://github.com/maxmind/" rel="nofollow" target="_blank">https://github.com/maxmind/</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.geoip.php)

**[To root](/README.md)**