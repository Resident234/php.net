# bzdecompress




<div class="phpcode"><span class="html">
I spent a while to sort out some integer results of the bzdecompress, so maybe it&apos;ll be useful for somebody else also...<br>(Constants from the sources.)<br><br>#define BZ_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0<br>#define BZ_RUN_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1<br>#define BZ_FLUSH_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 2<br>#define BZ_FINISH_OK&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 3<br>#define BZ_STREAM_END&#xA0; &#xA0; &#xA0; &#xA0; 4<br>#define BZ_SEQUENCE_ERROR&#xA0; &#xA0; (-1)<br>#define BZ_PARAM_ERROR&#xA0; &#xA0; &#xA0;&#xA0; (-2)<br>#define BZ_MEM_ERROR&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; (-3)<br>#define BZ_DATA_ERROR&#xA0; &#xA0; &#xA0; &#xA0; (-4)<br>#define BZ_DATA_ERROR_MAGIC&#xA0; (-5)<br>#define BZ_IO_ERROR&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (-6)<br>#define BZ_UNEXPECTED_EOF&#xA0; &#xA0; (-7)<br>#define BZ_OUTBUFF_FULL&#xA0; &#xA0; &#xA0; (-8)<br>#define BZ_CONFIG_ERROR&#xA0; &#xA0; &#xA0; (-9)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.bzdecompress.php)

**[To root](/README.md)**