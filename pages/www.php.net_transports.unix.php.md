# Unix Domain: Unix and UDG



It appears that fsockopen prior to php5 did not need the unix:// qualifier when opening a unix domain socket:<br><br>php4: fsockopen("/tmp/mysocket"......);

php5: fsockopen("unix:///tmp/mysocket"......);

This caught me out when upgrading.?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/transports.unix.php)

**[To root](/README.md)**