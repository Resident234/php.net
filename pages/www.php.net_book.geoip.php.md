# Geo IP Location



With GeoIP2, the easiest way is to:<br><br>* Grab the latest GeoIP2 Lite Database(s): https://dev.maxmind.com/geoip/geoip2/geolite2/<br>* Grab the latest geoip2.phar: https://github.com/maxmind/GeoIP2-php/releases<br><br>

```
<?php<br>require_once("geoip2.phar");<br>use GeoIp2\Database\Reader;<br>// City DB<br>$reader = new Reader(&apos;/path/to/GeoLite2-City.mmdb&apos;);<br>$record = $reader-&gt;city($_SERVER[&apos;REMOTE_ADDR&apos;]);<br>// or for Country DB<br>// $reader = new Reader(&apos;/path/to/GeoLite2-Country.mmdb&apos;);<br>// $record = $reader-&gt;country($_SERVER[&apos;REMOTE_ADDR&apos;]);<br>print($record-&gt;country-&gt;isoCode . "\n");<br>print($record-&gt;country-&gt;name . "\n");<br>print($record-&gt;country-&gt;names[&apos;zh-CN&apos;] . "\n");<br>print($record-&gt;mostSpecificSubdivision-&gt;name . "\n");<br>print($record-&gt;mostSpecificSubdivision-&gt;isoCode . "\n");<br>print($record-&gt;city-&gt;name . "\n");<br>print($record-&gt;postal-&gt;code . "\n");<br>print($record-&gt;location-&gt;latitude . "\n");<br>print($record-&gt;location-&gt;longitude . "\n");<br>$&gt;  

#

It should be noted that this extension has now been superseded by the GeoIP2 API that MaxMind now produces. There is a pure-PHP set of classes and a C library and extension you can optionally install. The code can be found in various projects on MaxMind&apos;s GitHub page: https://github.com/maxmind/  

#

[Official documentation page](https://www.php.net/manual/en/book.geoip.php)

**[To root](/README.md)**