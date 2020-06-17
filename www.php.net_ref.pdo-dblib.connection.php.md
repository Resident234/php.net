# PDO_DBLIB DSN




<div class="phpcode"><span class="html">
Instead of specifying tds version and client charset in freetds.conf, you may pass it as a parameter.<br><span class="default">&lt;?php $dsn </span><span class="keyword">= </span><span class="string">&apos;dblib:version=7.0;charset=UTF-8;host=domain.example.com;dbname=example;&apos;</span><span class="keyword">; </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re using FreeTDS driver and you want to use &quot;charset&quot; parameter then you may have to edit freetds.conf (e.g. /etc/freetds/freetds.conf) and force connection using at least version 7.0 of the protocol.<br><br>tds version = 7.0<br><br>Charset parameter accepts all encodings supported by iconv (execute iconv --list to show all encodings).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-dblib.connection.php)

**[To root](/README.md)**