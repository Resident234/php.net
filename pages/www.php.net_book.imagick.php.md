# Image Processing (ImageMagick)




<div class="phpcode"><span class="html">
Here is an example on how to take an image that is already in a string (say, from a database), and resize it, add a border, and print it out. I use this for showing reseller logos<br><br>&#xA0; // Decode image from base64<br>&#xA0; $image=base64_decode($imagedata);<br><br>&#xA0; // Create Imagick object<br>&#xA0; $im = new Imagick();<br><br>&#xA0; // Convert image into Imagick<br>&#xA0; $im-&gt;readimageblob($image);<br><br>&#xA0; // Create thumbnail max of 200x82<br>&#xA0; $im-&gt;thumbnailImage(200,82,true);<br><br>&#xA0; // Add a subtle border<br>&#xA0; $color=new ImagickPixel();<br>&#xA0; $color-&gt;setColor(&quot;rgb(220,220,220)&quot;);<br>&#xA0; $im-&gt;borderImage($color,1,1);<br><br>&#xA0; // Output the image<br>&#xA0; $output = $im-&gt;getimageblob();<br>&#xA0; $outputtype = $im-&gt;getFormat();<br><br>&#xA0; header(&quot;Content-type: $outputtype&quot;);<br>&#xA0; echo $output;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.imagick.php)

**[To root](/README.md)**