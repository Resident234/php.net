# imagejpeg



I didn&apos;t find any example like this on the Web, so I mixed pieces together.. hope this will save time to other people!<br><br>Here&apos;s a complete solution to READ any image (gif jpg png) from the FILESYSTEM, SCALE it to a max width/height, SAVE the scaled image to a BLOB field keeping the original image type. Quite tricky.. <br><br>

```
<?php

function scaleImageFileToBlob($file) {

    $source_pic = $file;
    $max_width = 200;
    $max_height = 200;

    list($width, $height, $image_type) = getimagesize($file);

    switch ($image_type)
    {
        case 1: $src = imagecreatefromgif($file); break;
        case 2: $src = imagecreatefromjpeg($file);  break;
        case 3: $src = imagecreatefrompng($file); break;
        default: return '';  break;
    }

    $x_ratio = $max_width / $width;
    $y_ratio = $max_height / $height;

    if( ($width <= $max_width) &amp;&amp; ($height <= $max_height) ){
        $tn_width = $width;
        $tn_height = $height;
        }elseif (($x_ratio * $height) < $max_height){
            $tn_height = ceil($x_ratio * $height);
            $tn_width = $max_width;
        }else{
            $tn_width = ceil($y_ratio * $width);
            $tn_height = $max_height;
    }

    $tmp = imagecreatetruecolor($tn_width,$tn_height);

    /* Check if this image is PNG or GIF, then set if Transparent*/
    if(($image_type == 1) OR ($image_type==3))
    {
        imagealphablending($tmp, false);
        imagesavealpha($tmp,true);
        $transparent = imagecolorallocatealpha($tmp, 255, 255, 255, 127);
        imagefilledrectangle($tmp, 0, 0, $tn_width, $tn_height, $transparent);
    }
    imagecopyresampled($tmp,$src,0,0,0,0,$tn_width, $tn_height,$width,$height);

    /*
     * imageXXX() only has two options, save as a file, or send to the browser.
     * It does not provide you the oppurtunity to manipulate the final GIF/JPG/PNG file stream
     * So I start the output buffering, use imageXXX() to output the data stream to the browser, 
     * get the contents of the stream, and use clean to silently discard the buffered contents.
     */
    ob_start();

    switch ($image_type)
    {
        case 1: imagegif($tmp); break;
        case 2: imagejpeg($tmp, NULL, 100);  break; // best quality
        case 3: imagepng($tmp, NULL, 0); break; // no compression
        default: echo ''; break;
    }

    $final_image = ob_get_contents();

    ob_end_clean();

    return $final_image;
}

?>
```


So, let's suppose you have a form where a user can upload an image, and you have to scale it and save it into your database.



```
<?php
    
    [..] // the user has clicked the Submit button..
    
    // Check if the user entered an image
    if ($_FILES['imagefile']['name'] != '') {
        $image = scaleImageFileToBlob($_FILES['imagefile']['tmp_name']);

        if ($image == '') {
            echo 'Image type not supported';
        } else {
            $image_type = $_FILES['imagefile']['type'];
            $image = addslashes($image);
            
            $query  = "UPDATE yourtable SET image_type='$image_type', image='$image' WHERE ...";
            $result = mysql_query($query);
            if ($result) {
               echo 'Image scaled and uploaded';
             } else {
               echo 'Error running the query';
             }
        }
    }

?>
```
  

---

If string $filename is given and it exists, it will be overwritten.  

---

[[Editor&apos;s note: removed the header()-call since it is not required when outputting inline image-data]]<br>One single code line, solve-me after 3 hours of blind search!<br><br>here is:<br><br>... ob_start();<br>  imagejpeg( $img, NULL, 100 );<br>  imagedestroy( $img );<br>  $i = ob_get_clean();<br><br>echo "&lt;img src=&apos;data:image/jpeg;base64," . base64_encode( $i )."&apos;&gt;"; //saviour line!  

---

[Official documentation page](https://www.php.net/manual/en/function.imagejpeg.php)

**[To root](/README.md)**