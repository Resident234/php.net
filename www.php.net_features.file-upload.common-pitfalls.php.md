# Common Pitfalls




<div class="phpcode"><span class="html">
Note that, when you want to upload VERY large files (or you want to set the limiters VERY high for test purposes), all of the upload file size limiters are stored in signed 32-bit ints.&#xA0; This means that setting a limit higher than about 2.1 GB will result in PHP seeing a large negative number.&#xA0; I have not found any way around this.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.common-pitfalls.php)

**[⬆ to root](/)**