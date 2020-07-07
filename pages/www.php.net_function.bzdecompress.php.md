# bzdecompress





I spent a while to sort out some integer results of the bzdecompress, so maybe it&apos;ll be useful for somebody else also...
(Constants from the sources.)

#define BZ_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0
#define BZ_RUN_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1
#define BZ_FLUSH_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 2
#define BZ_FINISH_OK&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 3
#define BZ_STREAM_END&#xA0; &#xA0; &#xA0; &#xA0; 4
#define BZ_SEQUENCE_ERROR&#xA0; &#xA0; (-1)
#define BZ_PARAM_ERROR&#xA0; &#xA0; &#xA0;&#xA0; (-2)
#define BZ_MEM_ERROR&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; (-3)
#define BZ_DATA_ERROR&#xA0; &#xA0; &#xA0; &#xA0; (-4)
#define BZ_DATA_ERROR_MAGIC&#xA0; (-5)
#define BZ_IO_ERROR&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (-6)
#define BZ_UNEXPECTED_EOF&#xA0; &#xA0; (-7)
#define BZ_OUTBUFF_FULL&#xA0; &#xA0; &#xA0; (-8)
#define BZ_CONFIG_ERROR&#xA0; &#xA0; &#xA0; (-9)

  

#

[Official documentation page](https://www.php.net/manual/en/function.bzdecompress.php)

**[To root](/README.md)**