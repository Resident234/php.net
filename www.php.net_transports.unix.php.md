# Unix Domain: Unix and UDG




<div class="phpcode"><span class="html">
It appears that fsockopen prior to php5 did not need the unix:// qualifier when opening a unix domain socket:<br><br>php4: fsockopen(&quot;/tmp/mysocket&quot;......);<br><br>php5: fsockopen(&quot;unix:///tmp/mysocket&quot;......);<br><br>This caught me out when upgrading.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/transports.unix.php)

**[⬆ to root](/)**