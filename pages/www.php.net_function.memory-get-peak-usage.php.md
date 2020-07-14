# memory_get_peak_usage



memory_get_peak_usage() is used to retrieve the highest memory usage of PHP (or your running script) only. If you need the overall memory usage of the entire system, following function might be helpful. If retrieves the memory usage either in percent (without the percent sign) or in bytes by returning an array with free and overall memory of your system. Tested with Windows (7) and Linux (on an Raspberry Pi 2):<br><br>

```
<?php

    // Returns used memory (either in percent (without percent sign) or free and overall in bytes)
    function getServerMemoryUsage($getPercentage=true)
    {
        $memoryTotal = null;
        $memoryFree = null;

        if (stristr(PHP_OS, "win")) {
            // Get total physical memory (this is in bytes)
            $cmd = "wmic ComputerSystem get TotalPhysicalMemory";
            @exec($cmd, $outputTotalPhysicalMemory);

            // Get free physical memory (this is in kibibytes!)
            $cmd = "wmic OS get FreePhysicalMemory";
            @exec($cmd, $outputFreePhysicalMemory);

            if ($outputTotalPhysicalMemory &amp;&amp; $outputFreePhysicalMemory) {
                // Find total value
                foreach ($outputTotalPhysicalMemory as $line) {
                    if ($line &amp;&amp; preg_match("/^[0-9]+\$/", $line)) {
                        $memoryTotal = $line;
                        break;
                    }
                }

                // Find free value
                foreach ($outputFreePhysicalMemory as $line) {
                    if ($line &amp;&amp; preg_match("/^[0-9]+\$/", $line)) {
                        $memoryFree = $line;
                        $memoryFree *= 1024;  // convert from kibibytes to bytes
                        break;
                    }
                }
            }
        }
        else
        {
            if (is_readable("/proc/meminfo"))
            {
                $stats = @file_get_contents("/proc/meminfo");

                if ($stats !== false) {
                    // Separate lines
                    $stats = str_replace(array("\r\n", "\n\r", "\r"), "\n", $stats);
                    $stats = explode("\n", $stats);

                    // Separate values and find correct lines for total and free mem
                    foreach ($stats as $statLine) {
                        $statLineData = explode(":", trim($statLine));

                        //
                        // Extract size (TODO: It seems that (at least) the two values for total and free memory have the unit "kB" always. Is this correct?
                        //

                        // Total memory
                        if (count($statLineData) == 2 &amp;&amp; trim($statLineData[0]) == "MemTotal") {
                            $memoryTotal = trim($statLineData[1]);
                            $memoryTotal = explode(" ", $memoryTotal);
                            $memoryTotal = $memoryTotal[0];
                            $memoryTotal *= 1024;  // convert from kibibytes to bytes
                        }

                        // Free memory
                        if (count($statLineData) == 2 &amp;&amp; trim($statLineData[0]) == "MemFree") {
                            $memoryFree = trim($statLineData[1]);
                            $memoryFree = explode(" ", $memoryFree);
                            $memoryFree = $memoryFree[0];
                            $memoryFree *= 1024;  // convert from kibibytes to bytes
                        }
                    }
                }
            }
        }

        if (is_null($memoryTotal) || is_null($memoryFree)) {
            return null;
        } else {
            if ($getPercentage) {
                return (100 - ($memoryFree * 100 / $memoryTotal));
            } else {
                return array(
                    "total" =&gt; $memoryTotal,
                    "free" =&gt; $memoryFree,
                );
            }
        }
    }

    function getNiceFileSize($bytes, $binaryPrefix=true) {
        if ($binaryPrefix) {
            $unit=array(&apos;B&apos;,&apos;KiB&apos;,&apos;MiB&apos;,&apos;GiB&apos;,&apos;TiB&apos;,&apos;PiB&apos;);
            if ($bytes==0) return &apos;0 &apos; . $unit[0];
            return @round($bytes/pow(1024,($i=floor(log($bytes,1024)))),2) .&apos; &apos;. (isset($unit[$i]) ? $unit[$i] : &apos;B&apos;);
        } else {
            $unit=array(&apos;B&apos;,&apos;KB&apos;,&apos;MB&apos;,&apos;GB&apos;,&apos;TB&apos;,&apos;PB&apos;);
            if ($bytes==0) return &apos;0 &apos; . $unit[0];
            return @round($bytes/pow(1000,($i=floor(log($bytes,1000)))),2) .&apos; &apos;. (isset($unit[$i]) ? $unit[$i] : &apos;B&apos;);
        }
    }

    // Memory usage: 4.55 GiB / 23.91 GiB (19.013557664178%)
    $memUsage = getServerMemoryUsage(false);
    echo sprintf("Memory usage: %s / %s (%s%%)",
        getNiceFileSize($memUsage["total"] - $memUsage["free"]),
        getNiceFileSize($memUsage["total"]),
        getServerMemoryUsage(true)
    );

?>
```
<br><br>The function getNiceFileSize() is not required. Just used to shorten size in bytes.<br><br>Note: If you need the server load (CPU usage), I wrote a nice function to get that too: http://php.net/manual/en/function.sys-getloadavg.php#118673  

#

[Official documentation page](https://www.php.net/manual/en/function.memory-get-peak-usage.php)

**[To root](/README.md)**