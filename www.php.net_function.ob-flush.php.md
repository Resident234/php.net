# ob_flush




<div class="phpcode"><span class="html">
some problems with ob_flush() and flush() could be resolved by defining content type header :<br>header( &apos;Content-type: text/html; charset=utf-8&apos; );<br><br>so working code looks like this:<br><span class="default">&lt;?php<br>header</span><span class="keyword">( </span><span class="string">&apos;Content-type: text/html; charset=utf-8&apos; </span><span class="keyword">);<br>echo </span><span class="string">&apos;Begin ...&lt;br /&gt;&apos;</span><span class="keyword">;<br>for( </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0 </span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">10 </span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++ )<br>{<br>&#xA0; &#xA0; echo </span><span class="default">$i </span><span class="keyword">. </span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">flush</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">ob_flush</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br>}<br>echo </span><span class="string">&apos;End ...&lt;br /&gt;&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As of August 2012, all browsers seem to show an all-or-nothing approach to buffering. In other words, while php is operating, no content can be shown.<br><br>In particular this means that the following workarounds listed further down here are ineffective:<br><br>1) ob_flush (),&#xA0; flush () in any combination with other output buffering functions;<br><br>2) changes to php.ini involving setting output_buffer and/or zlib.output_compression to 0 or Off;<br><br>3) setting Apache variables such as &quot;no-gzip&quot; either through apache_setenv () or through entries in .htaccess.<br><br>So, until browsers begin to show buffered content again, the tips listed here are moot.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Although browsers now have an all or none buffering strategy, the arguments are not moot.<br><br>If you are not using ob_flush, you run this risk of exceeding socket timeouts (commonly seen in php-fpm/nginx combos).<br><br>Basically, flushing solves the infamous 504 Gateway Time-out error.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-flush.php)

**[⬆ to root](/)**