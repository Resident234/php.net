# debug_zval_dump




<div class="phpcode"><span class="html">
&quot;Fatal error: Call-time pass-by-reference has been removed ...&quot; since PHP 5.something. So the Example #1 is now invalid PHP code.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The add of &quot;Call-time pass-by-reference&quot; as E_DEPRECATED makes it impossible to get the real refcount without getting an error, since <span class="default">&lt;?php error_reporting</span><span class="keyword">(</span><span class="default">E_ALL </span><span class="keyword">^ </span><span class="default">E_DEPRECATED</span><span class="keyword">); </span><span class="default">?&gt;</span> and even <span class="default">&lt;?php </span><span class="keyword">@</span><span class="default">debug_zval_dump</span><span class="keyword">(&amp;</span><span class="default">$foo</span><span class="keyword">); </span><span class="default">?&gt;</span> doesn&apos;t change anything.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.debug-zval-dump.php)

**[To root](/README.md)**