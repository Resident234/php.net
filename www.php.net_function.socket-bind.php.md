# socket_bind




<div class="phpcode"><span class="html">
If you want to reuse address and port, and get rid of error: unable to bind, address already in use, you have to use socket_setopt (check actual spelling for this function in you PHP verison) before calling bind:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if (!</span><span class="default">socket_set_option</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">, </span><span class="default">SOL_SOCKET</span><span class="keyword">, </span><span class="default">SO_REUSEADDR</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">)) {
<br>&#xA0; &#xA0; echo </span><span class="default">socket_strerror</span><span class="keyword">(</span><span class="default">socket_last_error</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">));
<br>&#xA0; &#xA0; exit;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>This solution was found by 
<br>Christophe Dirac. Thank you Christophe!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-bind.php)

**[⬆ to root](/)**