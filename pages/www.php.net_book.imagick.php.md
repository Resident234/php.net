# Image Processing (ImageMagick)





Here is an example on how to take an image that is already in a string (say, from a database), and resize it, add a border, and print it out. I use this for showing reseller logos

&#xA0; // Decode image from base64
&#xA0; $image=base64_decode($imagedata);

&#xA0; // Create Imagick object
&#xA0; $im = new Imagick();

&#xA0; // Convert image into Imagick
&#xA0; $im-&gt;readimageblob($image);

&#xA0; // Create thumbnail max of 200x82
&#xA0; $im-&gt;thumbnailImage(200,82,true);

&#xA0; // Add a subtle border
&#xA0; $color=new ImagickPixel();
&#xA0; $color-&gt;setColor(&quot;rgb(220,220,220)&quot;);
&#xA0; $im-&gt;borderImage($color,1,1);

&#xA0; // Output the image
&#xA0; $output = $im-&gt;getimageblob();
&#xA0; $outputtype = $im-&gt;getFormat();

&#xA0; header(&quot;Content-type: $outputtype&quot;);
&#xA0; echo $output;

  

#

[Official documentation page](https://www.php.net/manual/en/book.imagick.php)

**[To root](/README.md)**