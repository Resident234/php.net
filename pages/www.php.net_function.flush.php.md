# flush




<div class="phpcode"><span class="html">
This will show each line at a time with a pause of 2 seconds.<br>(Tested under IEx and Firefox)<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">if (</span><span class="default">ob_get_level</span><span class="keyword">() == </span><span class="default">0</span><span class="keyword">) </span><span class="default">ob_start</span><span class="keyword">();<br><br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">10</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++){<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;br&gt; Line to show.&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">str_pad</span><span class="keyword">(</span><span class="string">&apos;&apos;</span><span class="keyword">,</span><span class="default">4096</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;&#xA0; &#xA0; <br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ob_flush</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">flush</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">);<br>}<br><br>echo </span><span class="string">&quot;Done.&quot;</span><span class="keyword">;<br><br></span><span class="default">ob_end_flush</span><span class="keyword">();<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I would like to point out that there is a function to replace ob_flush and flush. If you set ob_implicit_flush(true); at the top of the page it will automatically flush any echo or print you do in the rest of the script.<br><br>Note that you still need a minimum amount of data to come through the browser filter. I would advice using str_pad($text,4096); since this automatically lenghtens the text with spaces to 4 KB which is the minimum limit when using FireFox and linux.<br><br>I hope this helps you all out a bit.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is what I use to turn off pretty much anything that could cause unwanted output buffering and turn on implicit flush:<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="keyword">@</span><span class="default">apache_setenv</span><span class="keyword">(</span><span class="string">&apos;no-gzip&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; @</span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;zlib.output_compression&apos;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br>&#xA0; &#xA0; @</span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;implicit_flush&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">ob_get_level</span><span class="keyword">(); </span><span class="default">$i</span><span class="keyword">++) { </span><span class="default">ob_end_flush</span><span class="keyword">(); }<br>&#xA0; &#xA0; </span><span class="default">ob_implicit_flush</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>If it still fails though, keep in mind that Internet Explorer and Safari have a 1k buffer before incremental rendering kicks in, so you&apos;ll want to output some padding as well.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For a Windows system using IIS, the ResponseBufferLimit takes precedence over PHP&apos;s output_buffering settings. So you must also set the ResponseBufferLimit to be something lower than its default value.<br><br>For IIS versions older than 7, the setting can be found in the %windir%\System32\inetsrv\fcgiext.ini file (the FastCGI config file). You can set the appropriate line to:<br>&#xA0; ResponseBufferLimit=0<br><br>For IIS 7+, the settings are stored in %windir%\System32\inetsrv\config. Edit the applicationHost.config file and search for PHP_via_FastCGI (assuming that you have installed PHP as a FastCGI module, as per the installation instructions, with the name PHP_via_FastCGI). Within the add tag, place the following setting at the end:<br>&#xA0; responseBufferLimit=&quot;0&quot;<br>So the entire line will look something like:<br>&#xA0; &lt;add name=&quot;PHP_via_FastCGI&quot; path=&quot;*.php&quot; verb=&quot;*&quot; modules=&quot;FastCgiModule&quot; scriptProcessor=&quot;C:\PHP\php-cgi.exe&quot; resourceType=&quot;Either&quot; responseBufferLimit=&quot;0&quot; /&gt;<br>Alternatively you can insert the setting using the following command:<br>&#xA0; %windir%\system32\inetsrv\appcmd.exe set config /section:handlers &quot;/[name=&apos;PHP_via_FastCGI&apos;].ResponseBufferLimit:0&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.flush.php)

**[To root](/README.md)**