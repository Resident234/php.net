# ob_get_level




<div class="phpcode"><span class="html">
For users confused about getting &quot;1&quot; as a return value from ob_get_level at the beginning of a script: this likely means the PHP ini directive &quot;output_buffering&quot; is not set to off / 0. PHP automatically starts output buffering for all your scripts if this directive is not off (which acts as if you called ob_start on the first line of your script).<br><br>If your scripts may end up on any server and you don&apos;t want end-users to have to configure their INI, you can use the following at the start of your script to stop output buffering if it&apos;s already started:<br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">ob_get_level</span><span class="keyword">()) </span><span class="default">ob_end_clean</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>Alternatively, you can use the opposite if you always want to have an output buffer at the start of your script:<br><span class="default">&lt;?php<br></span><span class="keyword">if (!</span><span class="default">ob_get_level</span><span class="keyword">()) </span><span class="default">ob_start</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-get-level.php)

**[To root](/)**