# imagecopyresampled



FOUR RECTANGLES<br><br>                  $src_image                                   $dst_image<br>+------------+---------------------------------+   +------------+--------------------+<br>|            |                                 |   |            |                    |<br>|            |                                 |   |         $dst_y                  |<br>|            |                                 |   |            |                    |<br>|         $src_y                               |   +-- $dst_x --+----$dst_width----+ |<br>|            |                                 |   |            |                  | |<br>|            |                                 |   |            |    Resampled     | |<br>|            |                                 |   |            |                  | |<br>+-- $src_x --+------ $src_width ------+        |   |       $dst_height             | |<br>|            |                        |        |   |            |                  | |<br>|            |                        |        |   |            |                  | |<br>|            |                        |        |   |            |                  | |<br>|            |                        |        |   |            +------------------+ |<br>|            |        Sample          |        |   |                                 |<br>|            |                        |        |   |                                 |<br>|            |                        |        |   |                                 |<br>|       $src_height                   |        |   |                                 |<br>|            |                        |        |   +---------------------------------+<br>|            |                        |        |<br>|            |                        |        |<br>|            +------------------------+        |<br>|                                              |<br>|                                              |<br>+----------------------------------------------+  

#

Here is my ultimate image resizer that preserves transparency for gif&apos;s and png&apos;s and has an option to crop images to fixed dimensions (preserves image proportions by default)<br><br>

```
<?php
function image_resize($src, $dst, $width, $height, $crop=0){

  if(!list($w, $h) = getimagesize($src)) return "Unsupported picture type!";

  $type = strtolower(substr(strrchr($src,"."),1));
  if($type == &apos;jpeg&apos;) $type = &apos;jpg&apos;;
  switch($type){
    case &apos;bmp&apos;: $img = imagecreatefromwbmp($src); break;
    case &apos;gif&apos;: $img = imagecreatefromgif($src); break;
    case &apos;jpg&apos;: $img = imagecreatefromjpeg($src); break;
    case &apos;png&apos;: $img = imagecreatefrompng($src); break;
    default : return "Unsupported picture type!";
  }

  // resize
  if($crop){
    if($w &lt; $width or $h &lt; $height) return "Picture is too small!";
    $ratio = max($width/$w, $height/$h);
    $h = $height / $ratio;
    $x = ($w - $width / $ratio) / 2;
    $w = $width / $ratio;
  }
  else{
    if($w &lt; $width and $h &lt; $height) return "Picture is too small!";
    $ratio = min($width/$w, $height/$h);
    $width = $w * $ratio;
    $height = $h * $ratio;
    $x = 0;
  }

  $new = imagecreatetruecolor($width, $height);

  // preserve transparency
  if($type == "gif" or $type == "png"){
    imagecolortransparent($new, imagecolorallocatealpha($new, 0, 0, 0, 127));
    imagealphablending($new, false);
    imagesavealpha($new, true);
  }

  imagecopyresampled($new, $img, 0, 0, $x, 0, $width, $height, $w, $h);

  switch($type){
    case &apos;bmp&apos;: imagewbmp($new, $dst); break;
    case &apos;gif&apos;: imagegif($new, $dst); break;
    case &apos;jpg&apos;: imagejpeg($new, $dst); break;
    case &apos;png&apos;: imagepng($new, $dst); break;
  }
  return true;
}
?>
```


Example that I use when uploading new images to the server.

This saves the original picture in the form:
original.type

and creates a new thumbnail:
100x100.type



```
<?php
  $pic_type = strtolower(strrchr($picture[&apos;name&apos;],"."));
  $pic_name = "original$pic_type";
  move_uploaded_file($picture[&apos;tmp_name&apos;], $pic_name);
  if (true !== ($pic_error = @image_resize($pic_name, "100x100$pic_type", 100, 100, 1))) {
    echo $pic_error;
    unlink($pic_name);
  }
  else echo "OK!";
?>
```
<br><br>Cheers!  

#

I&apos;ve created a PHP5 image resize class, using ImageCopyResampled, that someone might find useful, with support for JPEG, PNG, and GIF formats. It retains the original image&apos;s aspect ratio when resizing, and doesn&apos;t resize or resample if the original width and height is smaller then the desired resize.<br><br>

```
<?php

// Imaging
class imaging
{

    // Variables
    private $img_input;
    private $img_output;
    private $img_src;
    private $format;
    private $quality = 80;
    private $x_input;
    private $y_input;
    private $x_output;
    private $y_output;
    private $resize;

    // Set image
    public function set_img($img)
    {

        // Find format
        $ext = strtoupper(pathinfo($img, PATHINFO_EXTENSION));

        // JPEG image
        if(is_file($img) &amp;&amp; ($ext == "JPG" OR $ext == "JPEG"))
        {

            $this-&gt;format = $ext;
            $this-&gt;img_input = ImageCreateFromJPEG($img);
            $this-&gt;img_src = $img;
            

        }

        // PNG image
        elseif(is_file($img) &amp;&amp; $ext == "PNG")
        {

            $this-&gt;format = $ext;
            $this-&gt;img_input = ImageCreateFromPNG($img);
            $this-&gt;img_src = $img;

        }

        // GIF image
        elseif(is_file($img) &amp;&amp; $ext == "GIF")
        {

            $this-&gt;format = $ext;
            $this-&gt;img_input = ImageCreateFromGIF($img);
            $this-&gt;img_src = $img;

        }

        // Get dimensions
        $this-&gt;x_input = imagesx($this-&gt;img_input);
        $this-&gt;y_input = imagesy($this-&gt;img_input);

    }

    // Set maximum image size (pixels)
    public function set_size($size = 100)
    {

        // Resize
        if($this-&gt;x_input &gt; $size &amp;&amp; $this-&gt;y_input &gt; $size)
        {

            // Wide
            if($this-&gt;x_input &gt;= $this-&gt;y_input)
            {

                $this-&gt;x_output = $size;
                $this-&gt;y_output = ($this-&gt;x_output / $this-&gt;x_input) * $this-&gt;y_input;

            }

            // Tall
            else
            {

                $this-&gt;y_output = $size;
                $this-&gt;x_output = ($this-&gt;y_output / $this-&gt;y_input) * $this-&gt;x_input;

            }

            // Ready
            $this-&gt;resize = TRUE;

        }

        // Don&apos;t resize
        else { $this-&gt;resize = FALSE; }

    }

    // Set image quality (JPEG only)
    public function set_quality($quality)
    {

        if(is_int($quality))
        {

            $this-&gt;quality = $quality;

        }

    }

    // Save image
    public function save_img($path)
    {

        // Resize
        if($this-&gt;resize)
        {

            $this-&gt;img_output = ImageCreateTrueColor($this-&gt;x_output, $this-&gt;y_output);
            ImageCopyResampled($this-&gt;img_output, $this-&gt;img_input, 0, 0, 0, 0, $this-&gt;x_output, $this-&gt;y_output, $this-&gt;x_input, $this-&gt;y_input);

        }

        // Save JPEG
        if($this-&gt;format == "JPG" OR $this-&gt;format == "JPEG")
        {

            if($this-&gt;resize) { imageJPEG($this-&gt;img_output, $path, $this-&gt;quality); }
            else { copy($this-&gt;img_src, $path); }

        }

        // Save PNG
        elseif($this-&gt;format == "PNG")
        {

            if($this-&gt;resize) { imagePNG($this-&gt;img_output, $path); }
            else { copy($this-&gt;img_src, $path); }

        }

        // Save GIF
        elseif($this-&gt;format == "GIF")
        {

            if($this-&gt;resize) { imageGIF($this-&gt;img_output, $path); }
            else { copy($this-&gt;img_src, $path); }

        }

    }

    // Get width
    public function get_width()
    {

        return $this-&gt;x_input;

    }

    // Get height
    public function get_height()
    {

        return $this-&gt;y_input;

    }

    // Clear image cache
    public function clear_cache()
    {

        @ImageDestroy($this-&gt;img_input);
        @ImageDestroy($this-&gt;img_output);

    }

}

##### DEMO #####

// Image
$src = "myimage.jpg";

// Begin
$img = new imaging;
$img-&gt;set_img($src);
$img-&gt;set_quality(80);

// Small thumbnail
$img-&gt;set_size(200);
$img-&gt;save_img("small_" . $src);

// Baby thumbnail
$img-&gt;set_size(50);
$img-&gt;save_img("baby_" . $src);

// Finalize
$img-&gt;clear_cache();

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopyresampled.php)

**[To root](/README.md)**