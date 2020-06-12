# dns_get_record




<div class="phpcode"><span class="html">
This method has no error handling, it simply puts out &quot;false&quot; and it is impossible to check for NXDOMAIN, SERVFAIL, TIMEOUT or any other error...</span>
</div>
  

#


<div class="phpcode"><span class="html">
Get more than one type at once like this:<br><span class="default">&lt;?php<br>$dnsr </span><span class="keyword">= </span><span class="default">dns_get_record</span><span class="keyword">(</span><span class="string">&apos;php.net&apos;</span><span class="keyword">, </span><span class="default">DNS_A </span><span class="keyword">+ </span><span class="default">DNS_NS</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$dnsr</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Using DNS_ALL fails on some domains where DNS_ANY works. I noticed the function getting stuck on the DNS_PTR record, which caused it to return FALSE with this error:<br>PHP Warning:&#xA0; dns_get_record(): res_nsend() failed in ....<br><br>This gets all records except DNS_PTR:<br><span class="default">&lt;?php<br>$dnsr </span><span class="keyword">= </span><span class="default">dns_get_record</span><span class="keyword">(</span><span class="string">&apos;php.net&apos;</span><span class="keyword">, </span><span class="default">DNS_ALL </span><span class="keyword">- </span><span class="default">DNS_PTR</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$dnsr</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.dns-get-record.php)

**[⬆ to root](/)**