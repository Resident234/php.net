# memory_get_usage





To get the memory usage in KB or MB



```
<?php
function convert($size)
 {
&#xA0; &#xA0; $unit=array(&apos;b&apos;,&apos;kb&apos;,&apos;mb&apos;,&apos;gb&apos;,&apos;tb&apos;,&apos;pb&apos;);
&#xA0; &#xA0; return @round($size/pow(1024,($i=floor(log($size,1024)))),2).&apos; &apos;.$unit[$i];
 }

echo convert(memory_get_usage(true)); // 123 kb
?>
```



  

#



Note, that the official IEC-prefix for kilobyte, megabyte and so on are KiB, MiB, TiB and so on.

See http://en.wikipedia.org/wiki/Tebibyte

At first glance this may sound like &quot;What the hell? Everybody knows, that we mean 1024 not 1000 and the difference is not too big, so what?&quot;. But in about 10 years, the size of harddisks (and files on them) reaches the petabyte-limit and then the difference between PB and PiB is magnificent.

Better to get used to it now. :)

  

#



memory_get_usage() is used to retrieve the memory allocated to PHP only (or your running script). But intuitively, many people expect to get the memory usage of the system, based on the name of the function.

So if you need the overall memory usage, following function might be helpful. If retrieves the memory usage either in percent (without the percent sign) or in bytes by returning an array with free and overall memory of your system. Tested with Windows (7) and Linux (on an Raspberry Pi 2):



```
<?php

&#xA0; &#xA0; // Returns used memory (either in percent (without percent sign) or free and overall in bytes)
&#xA0; &#xA0; function getServerMemoryUsage($getPercentage=true)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $memoryTotal = null;
&#xA0; &#xA0; &#xA0; &#xA0; $memoryFree = null;

&#xA0; &#xA0; &#xA0; &#xA0; if (stristr(PHP_OS, &quot;win&quot;)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Get total physical memory (this is in bytes)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cmd = &quot;wmic ComputerSystem get TotalPhysicalMemory&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @exec($cmd, $outputTotalPhysicalMemory);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Get free physical memory (this is in kibibytes!)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cmd = &quot;wmic OS get FreePhysicalMemory&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @exec($cmd, $outputFreePhysicalMemory);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($outputTotalPhysicalMemory &amp;&amp; $outputFreePhysicalMemory) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Find total value
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($outputTotalPhysicalMemory as $line) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($line &amp;&amp; preg_match(&quot;/^[0-9]+\$/&quot;, $line)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryTotal = $line;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Find free value
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($outputFreePhysicalMemory as $line) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($line &amp;&amp; preg_match(&quot;/^[0-9]+\$/&quot;, $line)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryFree = $line;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryFree *= 1024;&#xA0; // convert from kibibytes to bytes
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (is_readable(&quot;/proc/meminfo&quot;))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stats = @file_get_contents(&quot;/proc/meminfo&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($stats !== false) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Separate lines
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stats = str_replace(array(&quot;\r\n&quot;, &quot;\n\r&quot;, &quot;\r&quot;), &quot;\n&quot;, $stats);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stats = explode(&quot;\n&quot;, $stats);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Separate values and find correct lines for total and free mem
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($stats as $statLine) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $statLineData = explode(&quot;:&quot;, trim($statLine));

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Extract size (TODO: It seems that (at least) the two values for total and free memory have the unit &quot;kB&quot; always. Is this correct?
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Total memory
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (count($statLineData) == 2 &amp;&amp; trim($statLineData[0]) == &quot;MemTotal&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryTotal = trim($statLineData[1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryTotal = explode(&quot; &quot;, $memoryTotal);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryTotal = $memoryTotal[0];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryTotal *= 1024;&#xA0; // convert from kibibytes to bytes
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Free memory
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (count($statLineData) == 2 &amp;&amp; trim($statLineData[0]) == &quot;MemFree&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryFree = trim($statLineData[1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryFree = explode(&quot; &quot;, $memoryFree);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryFree = $memoryFree[0];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $memoryFree *= 1024;&#xA0; // convert from kibibytes to bytes
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; if (is_null($memoryTotal) || is_null($memoryFree)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return null;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($getPercentage) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return (100 - ($memoryFree * 100 / $memoryTotal));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;total&quot; =&gt; $memoryTotal,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;free&quot; =&gt; $memoryFree,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; function getNiceFileSize($bytes, $binaryPrefix=true) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($binaryPrefix) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $unit=array(&apos;B&apos;,&apos;KiB&apos;,&apos;MiB&apos;,&apos;GiB&apos;,&apos;TiB&apos;,&apos;PiB&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($bytes==0) return &apos;0 &apos; . $unit[0];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return @round($bytes/pow(1024,($i=floor(log($bytes,1024)))),2) .&apos; &apos;. (isset($unit[$i]) ? $unit[$i] : &apos;B&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $unit=array(&apos;B&apos;,&apos;KB&apos;,&apos;MB&apos;,&apos;GB&apos;,&apos;TB&apos;,&apos;PB&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($bytes==0) return &apos;0 &apos; . $unit[0];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return @round($bytes/pow(1000,($i=floor(log($bytes,1000)))),2) .&apos; &apos;. (isset($unit[$i]) ? $unit[$i] : &apos;B&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; // Memory usage: 4.55 GiB / 23.91 GiB (19.013557664178%)
&#xA0; &#xA0; $memUsage = getServerMemoryUsage(false);
&#xA0; &#xA0; echo sprintf(&quot;Memory usage: %s / %s (%s%%)&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; getNiceFileSize($memUsage[&quot;total&quot;] - $memUsage[&quot;free&quot;]),
&#xA0; &#xA0; &#xA0; &#xA0; getNiceFileSize($memUsage[&quot;total&quot;]),
&#xA0; &#xA0; &#xA0; &#xA0; getServerMemoryUsage(true)
&#xA0; &#xA0; );

?>
```


The function getNiceFileSize() is not required. Just used to shorten size in bytes.

Note: If you need the server load (CPU usage), I wrote a nice function to get that too: http://php.net/manual/en/function.sys-getloadavg.php#118673

  

#



To get the memory usage in KB or MB





```
<?php

&#xA0; &#xA0; function echo_memory_usage() {

&#xA0; &#xA0; &#xA0; &#xA0; $mem_usage = memory_get_usage(true);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; if ($mem_usage &lt; 1024)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo $mem_usage.&quot; bytes&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; elseif ($mem_usage &lt; 1048576)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo round($mem_usage/1024,2).&quot; kilobytes&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; else

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo round($mem_usage/1048576,2).&quot; megabytes&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;br/&gt;&quot;;

&#xA0; &#xA0; }

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.memory-get-usage.php)

**[To root](/README.md)**