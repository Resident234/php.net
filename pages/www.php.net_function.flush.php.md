# flush





This will show each line at a time with a pause of 2 seconds.
(Tested under IEx and Firefox)



```
<?php

if (ob_get_level() == 0) ob_start();

for ($i = 0; $i&lt;10; $i++){

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;br&gt; Line to show.&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; echo str_pad(&apos;&apos;,4096).&quot;\n&quot;;&#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; ob_flush();
&#xA0; &#xA0; &#xA0; &#xA0; flush();
&#xA0; &#xA0; &#xA0; &#xA0; sleep(2);
}

echo &quot;Done.&quot;;

ob_end_flush();

?>
```



  

#



I would like to point out that there is a function to replace ob_flush and flush. If you set ob_implicit_flush(true); at the top of the page it will automatically flush any echo or print you do in the rest of the script.

Note that you still need a minimum amount of data to come through the browser filter. I would advice using str_pad($text,4096); since this automatically lenghtens the text with spaces to 4 KB which is the minimum limit when using FireFox and linux.

I hope this helps you all out a bit.

  

#



This is what I use to turn off pretty much anything that could cause unwanted output buffering and turn on implicit flush:



```
<?php

&#xA0; &#xA0; @apache_setenv(&apos;no-gzip&apos;, 1);
&#xA0; &#xA0; @ini_set(&apos;zlib.output_compression&apos;, 0);
&#xA0; &#xA0; @ini_set(&apos;implicit_flush&apos;, 1);
&#xA0; &#xA0; for ($i = 0; $i &lt; ob_get_level(); $i++) { ob_end_flush(); }
&#xA0; &#xA0; ob_implicit_flush(1);

?>
```


If it still fails though, keep in mind that Internet Explorer and Safari have a 1k buffer before incremental rendering kicks in, so you&apos;ll want to output some padding as well.

  

#



For a Windows system using IIS, the ResponseBufferLimit takes precedence over PHP&apos;s output_buffering settings. So you must also set the ResponseBufferLimit to be something lower than its default value.

For IIS versions older than 7, the setting can be found in the %windir%\System32\inetsrv\fcgiext.ini file (the FastCGI config file). You can set the appropriate line to:
&#xA0; ResponseBufferLimit=0

For IIS 7+, the settings are stored in %windir%\System32\inetsrv\config. Edit the applicationHost.config file and search for PHP_via_FastCGI (assuming that you have installed PHP as a FastCGI module, as per the installation instructions, with the name PHP_via_FastCGI). Within the add tag, place the following setting at the end:
&#xA0; responseBufferLimit=&quot;0&quot;
So the entire line will look something like:
&#xA0; &lt;add name=&quot;PHP_via_FastCGI&quot; path=&quot;*.php&quot; verb=&quot;*&quot; modules=&quot;FastCgiModule&quot; scriptProcessor=&quot;C:\PHP\php-cgi.exe&quot; resourceType=&quot;Either&quot; responseBufferLimit=&quot;0&quot; /&gt;
Alternatively you can insert the setting using the following command:
&#xA0; %windir%\system32\inetsrv\appcmd.exe set config /section:handlers &quot;/[name=&apos;PHP_via_FastCGI&apos;].ResponseBufferLimit:0&quot;

  

#

[Official documentation page](https://www.php.net/manual/en/function.flush.php)

**[To root](/README.md)**