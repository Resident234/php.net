# dl




<div class="phpcode"><span class="html">
dl is awkward because the filename format is OS-dependent and because it can complain if the extension is already loaded. This wrapper function fixes that:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">load_lib</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">, </span><span class="default">$f </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="default">extension_loaded</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">) or </span><span class="default">dl</span><span class="keyword">(((</span><span class="default">PHP_SHLIB_SUFFIX </span><span class="keyword">=== </span><span class="string">&apos;dll&apos;</span><span class="keyword">) ? </span><span class="string">&apos;php_&apos; </span><span class="keyword">: </span><span class="string">&apos;&apos;</span><span class="keyword">) . (</span><span class="default">$f </span><span class="keyword">? </span><span class="default">$f </span><span class="keyword">: </span><span class="default">$n</span><span class="keyword">) . </span><span class="string">&apos;.&apos; </span><span class="keyword">. </span><span class="default">PHP_SHLIB_SUFFIX</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;<br></span><br>Examples:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// ensure we have SSL and MySQL support<br></span><span class="default">load_lib</span><span class="keyword">(</span><span class="string">&apos;openssl&apos;</span><span class="keyword">);<br></span><span class="default">load_lib</span><span class="keyword">(</span><span class="string">&apos;mysql&apos;</span><span class="keyword">);<br><br></span><span class="comment">// a rare few extensions have a different filename to their extension name, such as the image (gd) library, so we specify them like this:<br></span><span class="default">load_lib</span><span class="keyword">(</span><span class="string">&apos;gd&apos;</span><span class="keyword">, </span><span class="string">&apos;gd2&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It would be nice to know which SAPIs removed the function. <br><br>Telling me &apos;This function has been removed from some SAPIs in PHP 5.3.&apos; is pretty much useless and I feel mocked. Or do the writers of the documentation don&apos;t know from which SAPIs it has been removed?</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.dl.php)

**[To root](/)**