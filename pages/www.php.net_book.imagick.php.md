# Image Processing (ImageMagick)



Here is an example on how to take an image that is already in a string (say, from a database), and resize it, add a border, and print it out. I use this for showing reseller logos<br><br>  // Decode image from base64<br>  $image=base64_decode($imagedata);<br><br>  // Create Imagick object<br>  $im = new Imagick();<br><br>  // Convert image into Imagick<br>  $im-&gt;readimageblob($image);<br><br>  // Create thumbnail max of 200x82<br>  $im-&gt;thumbnailImage(200,82,true);<br><br>  // Add a subtle border<br>  $color=new ImagickPixel();<br>  $color-&gt;setColor("rgb(220,220,220)");<br>  $im-&gt;borderImage($color,1,1);<br><br>  // Output the image<br>  $output = $im-&gt;getimageblob();<br>  $outputtype = $im-&gt;getFormat();<br><br>  header("Content-type: $outputtype");<br>  echo $output;  

---

[Official documentation page](https://www.php.net/manual/en/book.imagick.php)

**[To root](/README.md)**