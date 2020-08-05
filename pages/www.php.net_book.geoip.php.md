# Geo IP Location



With GeoIP2, the easiest way is to:<br><br>* Grab the latest GeoIP2 Lite Database(s): https://dev.maxmind.com/geoip/geoip2/geolite2/<br>* Grab the latest geoip2.phar: https://github.com/maxmind/GeoIP2-php/releases<br><br>

```
<?php
require_once("geoip2.phar");
use GeoIp2\Database\Reader;
// City DB
$reader = new Reader('/path/to/GeoLite2-City.mmdb');
$record = $reader->city($_SERVER['REMOTE_ADDR']);
// or for Country DB
// $reader = new Reader('/path/to/GeoLite2-Country.mmdb');
// $record = $reader->country($_SERVER['REMOTE_ADDR']);
print($record->country->isoCode . "\n");
print($record->country->name . "\n");
print($record->country->names['zh-CN'] . "\n");
print($record->mostSpecificSubdivision->name . "\n");
print($record->mostSpecificSubdivision->isoCode . "\n");
print($record->city->name . "\n");
print($record->postal->code . "\n");
print($record->location->latitude . "\n");
print($record->location->longitude . "\n");
$>?>
```
  

---

It should be noted that this extension has now been superseded by the GeoIP2 API that MaxMind now produces. There is a pure-PHP set of classes and a C library and extension you can optionally install. The code can be found in various projects on MaxMind&apos;s GitHub page: https://github.com/maxmind/  

---

[Official documentation page](https://www.php.net/manual/en/book.geoip.php)

**[To root](/README.md)**