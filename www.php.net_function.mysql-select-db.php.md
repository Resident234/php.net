# mysql_select_db




<div class="phpcode"><span class="html">
Be carefull if you are using two databases on the same server at the same time.&#xA0; By default mysql_connect returns the same connection ID for multiple calls with the same server parameters, which means if you do <br><br><span class="default">&lt;?php<br>&#xA0; $db1 </span><span class="keyword">= </span><span class="default">mysql_connect</span><span class="keyword">(...</span><span class="default">stuff</span><span class="keyword">...);<br>&#xA0; </span><span class="default">$db2 </span><span class="keyword">= </span><span class="default">mysql_connect</span><span class="keyword">(...</span><span class="default">stuff</span><span class="keyword">...);<br>&#xA0; </span><span class="default">mysql_select_db</span><span class="keyword">(</span><span class="string">&apos;db1&apos;</span><span class="keyword">, </span><span class="default">$db1</span><span class="keyword">);<br>&#xA0; </span><span class="default">mysql_select_db</span><span class="keyword">(</span><span class="string">&apos;db2&apos;</span><span class="keyword">, </span><span class="default">$db2</span><span class="keyword">); <br></span><span class="default">?&gt;<br></span><br>then $db1 will actually have selected the database &apos;db2&apos;, because the second call to mysql_connect just returned the already opened connection ID !<br><br>You have two options here, eiher you have to call mysql_select_db before each query you do, or if you&apos;re using php4.2+ there is a parameter to mysql_connect to force the creation of a new link.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-select-db.php)

**[To root](/README.md)**