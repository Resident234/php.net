# imagejpeg





I didn&apos;t find any example like this on the Web, so I mixed pieces together.. hope this will save time to other people!

Here&apos;s a complete solution to READ any image (gif jpg png) from the FILESYSTEM, SCALE it to a max width/height, SAVE the scaled image to a BLOB field keeping the original image type. Quite tricky.. 



```
<?php

function scaleImageFileToBlob($file) {

&#xA0; &#xA0; $source_pic = $file;
&#xA0; &#xA0; $max_width = 200;
&#xA0; &#xA0; $max_height = 200;

&#xA0; &#xA0; list($width, $height, $image_type) = getimagesize($file);

&#xA0; &#xA0; switch ($image_type)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; case 1: $src = imagecreatefromgif($file); break;
&#xA0; &#xA0; &#xA0; &#xA0; case 2: $src = imagecreatefromjpeg($file);&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 3: $src = imagecreatefrompng($file); break;
&#xA0; &#xA0; &#xA0; &#xA0; default: return &apos;&apos;;&#xA0; break;
&#xA0; &#xA0; }

&#xA0; &#xA0; $x_ratio = $max_width / $width;
&#xA0; &#xA0; $y_ratio = $max_height / $height;

&#xA0; &#xA0; if( ($width &lt;= $max_width) &amp;&amp; ($height &lt;= $max_height) ){
&#xA0; &#xA0; &#xA0; &#xA0; $tn_width = $width;
&#xA0; &#xA0; &#xA0; &#xA0; $tn_height = $height;
&#xA0; &#xA0; &#xA0; &#xA0; }elseif (($x_ratio * $height) &lt; $max_height){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tn_height = ceil($x_ratio * $height);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tn_width = $max_width;
&#xA0; &#xA0; &#xA0; &#xA0; }else{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tn_width = ceil($y_ratio * $width);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tn_height = $max_height;
&#xA0; &#xA0; }

&#xA0; &#xA0; $tmp = imagecreatetruecolor($tn_width,$tn_height);

&#xA0; &#xA0; /* Check if this image is PNG or GIF, then set if Transparent*/
&#xA0; &#xA0; if(($image_type == 1) OR ($image_type==3))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; imagealphablending($tmp, false);
&#xA0; &#xA0; &#xA0; &#xA0; imagesavealpha($tmp,true);
&#xA0; &#xA0; &#xA0; &#xA0; $transparent = imagecolorallocatealpha($tmp, 255, 255, 255, 127);
&#xA0; &#xA0; &#xA0; &#xA0; imagefilledrectangle($tmp, 0, 0, $tn_width, $tn_height, $transparent);
&#xA0; &#xA0; }
&#xA0; &#xA0; imagecopyresampled($tmp,$src,0,0,0,0,$tn_width, $tn_height,$width,$height);

&#xA0; &#xA0; /*
&#xA0; &#xA0;&#xA0; * imageXXX() only has two options, save as a file, or send to the browser.
&#xA0; &#xA0;&#xA0; * It does not provide you the oppurtunity to manipulate the final GIF/JPG/PNG file stream
&#xA0; &#xA0;&#xA0; * So I start the output buffering, use imageXXX() to output the data stream to the browser, 
&#xA0; &#xA0;&#xA0; * get the contents of the stream, and use clean to silently discard the buffered contents.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; ob_start();

&#xA0; &#xA0; switch ($image_type)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; case 1: imagegif($tmp); break;
&#xA0; &#xA0; &#xA0; &#xA0; case 2: imagejpeg($tmp, NULL, 100);&#xA0; break; // best quality
&#xA0; &#xA0; &#xA0; &#xA0; case 3: imagepng($tmp, NULL, 0); break; // no compression
&#xA0; &#xA0; &#xA0; &#xA0; default: echo &apos;&apos;; break;
&#xA0; &#xA0; }

&#xA0; &#xA0; $final_image = ob_get_contents();

&#xA0; &#xA0; ob_end_clean();

&#xA0; &#xA0; return $final_image;
}

?>
```


So, let&apos;s suppose you have a form where a user can upload an image, and you have to scale it and save it into your database.



```
<?php
&#xA0; &#xA0; 
&#xA0; &#xA0; [..] // the user has clicked the Submit button..
&#xA0; &#xA0; 
&#xA0; &#xA0; // Check if the user entered an image
&#xA0; &#xA0; if ($_FILES[&apos;imagefile&apos;][&apos;name&apos;] != &apos;&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; $image = scaleImageFileToBlob($_FILES[&apos;imagefile&apos;][&apos;tmp_name&apos;]);

&#xA0; &#xA0; &#xA0; &#xA0; if ($image == &apos;&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Image type not supported&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image_type = $_FILES[&apos;imagefile&apos;][&apos;type&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image = addslashes($image);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $query&#xA0; = &quot;UPDATE yourtable SET image_type=&apos;$image_type&apos;, image=&apos;$image&apos; WHERE ...&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = mysql_query($query);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($result) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; echo &apos;Image scaled and uploaded&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; echo &apos;Error running the query&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

?>
```



  

#



If string $filename is given and it exists, it will be overwritten.

  

#



[[Editor&apos;s note: removed the header()-call since it is not required when outputting inline image-data]]

One single code line, solve-me after 3 hours of blind search!



here is:



... ob_start();

&#xA0; imagejpeg( $img, NULL, 100 );

&#xA0; imagedestroy( $img );

&#xA0; $i = ob_get_clean();



echo &quot;&lt;img src=&apos;data:image/jpeg;base64,&quot; . base64_encode( $i ).&quot;&apos;&gt;&quot;; //saviour line!

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagejpeg.php)

**[To root](/README.md)**