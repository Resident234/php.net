# flush



This will show each line at a time with a pause of 2 seconds.<br>(Tested under IEx and Firefox)<br><br>

```
<?php

if (ob_get_level() == 0) ob_start();

for ($i = 0; $i&lt;10; $i++){

        echo "&lt;br&gt; Line to show.";
        echo str_pad('',4096)."\n";    

        ob_flush();
        flush();
        sleep(2);
}

echo "Done.";

ob_end_flush();

?>
```
  

#

I would like to point out that there is a function to replace ob_flush and flush. If you set ob_implicit_flush(true); at the top of the page it will automatically flush any echo or print you do in the rest of the script.<br><br>Note that you still need a minimum amount of data to come through the browser filter. I would advice using str_pad($text,4096); since this automatically lenghtens the text with spaces to 4 KB which is the minimum limit when using FireFox and linux.<br><br>I hope this helps you all out a bit.  

#

This is what I use to turn off pretty much anything that could cause unwanted output buffering and turn on implicit flush:<br><br>

```
<?php

    @apache_setenv('no-gzip', 1);
    @ini_set('zlib.output_compression', 0);
    @ini_set('implicit_flush', 1);
    for ($i = 0; $i &lt; ob_get_level(); $i++) { ob_end_flush(); }
    ob_implicit_flush(1);

?>
```
<br><br>If it still fails though, keep in mind that Internet Explorer and Safari have a 1k buffer before incremental rendering kicks in, so you&apos;ll want to output some padding as well.  

#

For a Windows system using IIS, the ResponseBufferLimit takes precedence over PHP&apos;s output_buffering settings. So you must also set the ResponseBufferLimit to be something lower than its default value.<br><br>For IIS versions older than 7, the setting can be found in the %windir%\System32\inetsrv\fcgiext.ini file (the FastCGI config file). You can set the appropriate line to:<br>  ResponseBufferLimit=0<br><br>For IIS 7+, the settings are stored in %windir%\System32\inetsrv\config. Edit the applicationHost.config file and search for PHP_via_FastCGI (assuming that you have installed PHP as a FastCGI module, as per the installation instructions, with the name PHP_via_FastCGI). Within the add tag, place the following setting at the end:<br>  responseBufferLimit="0"<br>So the entire line will look something like:<br>  &lt;add name="PHP_via_FastCGI" path="*.php" verb="*" modules="FastCgiModule" scriptProcessor="C:\PHP\?>
```
cgi.exe" resourceType="Either" responseBufferLimit="0" /&gt;<br>Alternatively you can insert the setting using the following command:<br>  %windir%\system32\inetsrv\appcmd.exe set config /section:handlers "/[name=&apos;PHP_via_FastCGI&apos;].ResponseBufferLimit:0"  

#

[Official documentation page](https://www.php.net/manual/en/function.flush.php)

**[To root](/README.md)**