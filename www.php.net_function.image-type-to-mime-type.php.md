# image_type_to_mime_type




<div class="phpcode"><span class="html">
If you are working with Images only and you need mime type (e.g. for headers), then this is a fast and reliable technique:<br> <br><span class="default">&lt;?php<br>$file </span><span class="keyword">= </span><span class="string">&apos;path/to/image.jpg&apos;</span><span class="keyword">;<br></span><span class="default">$image_mime </span><span class="keyword">= </span><span class="default">image_type_to_mime_type</span><span class="keyword">(</span><span class="default">exif_imagetype</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>It will output true image mime type even if you rename your image file.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.image-type-to-mime-type.php)

**[⬆ to root](/)**