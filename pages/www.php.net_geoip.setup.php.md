# Installing/Configuring



As of January 2, 2019 Maxmind discontinued the original GeoLite databases that we have been using in all these examples. You can read the full announcement here: https://support.maxmind.com/geolite-legacy-discontinuation-notice/<br><br>This means that if you require to have an updated version of the GeoIP information, there are a few steps you need to do:<br>1. Install the GeoIP2 client libraries (Composer, Phar)<br>2. Download the new GeoIP2 datasets<br>3. Update your application to make use of this new API and dataset<br><br>You can find detailed instructions on http://maxmind.github.io/GeoIP2-php/<br><br>C extension<br>Yes, there is a C extension available! Check out https://github.com/maxmind/MaxMind-DB-Reader-php for more details on how to install and use this extension.  

#

To install geoip on debian lenny:<br><br>wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz<br>gunzip GeoLiteCity.dat.gz<br>sudo mkdir -v /usr/share/GeoIP<br>sudo mv -v GeoLiteCity.dat /usr/share/GeoIP/GeoIPCity.dat<br><br>sudo apt-get install php5-geoip<br><br>and then try it in PHP:<br>print_r(geoip_record_by_name(&apos;php.net&apos;));<br><br>returns:<br>Array<br>(<br>    [country_code] =&gt; US<br>    [country_code3] =&gt; USA<br>    [country_name] =&gt; United States<br>    [region] =&gt; CA<br>    [city] =&gt; Sunnyvale<br>    [postal_code] =&gt; 94089<br>    [latitude] =&gt; 37.4249000549<br>    [longitude] =&gt; -122.007400513<br>    [dma_code] =&gt; 807<br>    [area_code] =&gt; 408<br>)  

#

[Official documentation page](https://www.php.net/manual/en/geoip.setup.php)

**[To root](/README.md)**