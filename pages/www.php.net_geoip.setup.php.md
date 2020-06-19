# Installing/Configuring




<div class="phpcode"><span class="html">
As of January 2, 2019 Maxmind discontinued the original GeoLite databases that we have been using in all these examples. You can read the full announcement here: <a href="https://support.maxmind.com/geolite-legacy-discontinuation-notice/" rel="nofollow" target="_blank">https://support.maxmind.com/geolite-legacy-discontinuation-notice/</a><br><br>This means that if you require to have an updated version of the GeoIP information, there are a few steps you need to do:<br>1. Install the GeoIP2 client libraries (Composer, Phar)<br>2. Download the new GeoIP2 datasets<br>3. Update your application to make use of this new API and dataset<br><br>You can find detailed instructions on <a href="http://maxmind.github.io/GeoIP2-php/" rel="nofollow" target="_blank">http://maxmind.github.io/GeoIP2-php/</a><br><br>C extension<br>Yes, there is a C extension available! Check out <a href="https://github.com/maxmind/MaxMind-DB-Reader-php" rel="nofollow" target="_blank">https://github.com/maxmind/MaxMind-DB-Reader-php</a> for more details on how to install and use this extension.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To install geoip on debian lenny:<br><br>wget <a href="http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz" rel="nofollow" target="_blank">http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz</a><br>gunzip GeoLiteCity.dat.gz<br>sudo mkdir -v /usr/share/GeoIP<br>sudo mv -v GeoLiteCity.dat /usr/share/GeoIP/GeoIPCity.dat<br><br>sudo apt-get install php5-geoip<br><br>and then try it in PHP:<br>print_r(geoip_record_by_name(&apos;php.net&apos;));<br><br>returns:<br>Array<br>(<br>&#xA0; &#xA0; [country_code] =&gt; US<br>&#xA0; &#xA0; [country_code3] =&gt; USA<br>&#xA0; &#xA0; [country_name] =&gt; United States<br>&#xA0; &#xA0; [region] =&gt; CA<br>&#xA0; &#xA0; [city] =&gt; Sunnyvale<br>&#xA0; &#xA0; [postal_code] =&gt; 94089<br>&#xA0; &#xA0; [latitude] =&gt; 37.4249000549<br>&#xA0; &#xA0; [longitude] =&gt; -122.007400513<br>&#xA0; &#xA0; [dma_code] =&gt; 807<br>&#xA0; &#xA0; [area_code] =&gt; 408<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/geoip.setup.php)

**[To root](/README.md)**