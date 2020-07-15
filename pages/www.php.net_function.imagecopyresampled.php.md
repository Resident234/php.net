# imagecopyresampled



FOUR RECTANGLES<br><br>                  $src_image                                   $dst_image<br>+------------+---------------------------------+   +------------+--------------------+<br>|            |                                 |   |            |                    |<br>|            |                                 |   |         $dst_y                  |<br>|            |                                 |   |            |                    |<br>|         $src_y                               |   +-- $dst_x --+----$dst_width----+ |<br>|            |                                 |   |            |                  | |<br>|            |                                 |   |            |    Resampled     | |<br>|            |                                 |   |            |                  | |<br>+-- $src_x --+------ $src_width ------+        |   |       $dst_height             | |<br>|            |                        |        |   |            |                  | |<br>|            |                        |        |   |            |                  | |<br>|            |                        |        |   |            |                  | |<br>|            |                        |        |   |            +------------------+ |<br>|            |        Sample          |        |   |                                 |<br>|            |                        |        |   |                                 |<br>|            |                        |        |   |                                 |<br>|       $src_height                   |        |   |                                 |<br>|            |                        |        |   +---------------------------------+<br>|            |                        |        |<br>|            |                        |        |<br>|            +------------------------+        |<br>|                                              |<br>|                                              |<br>+----------------------------------------------+  

#

Here is my ultimate image resizer that preserves transparency for gif&apos;s and png&apos;s and has an option to crop images to fixed dimensions (preserves image proportions by default)<br><br>

```
<?php
function image_resize($src, $dst, $width, $height, $crop=0){

  if(!list($w, $h) = getimagesize($src)) return "Unsupported picture type!";

  $type = strtolower(substr(strrchr($src,"."),1));
  if($type == 'jpeg') $type = 'jpg';
  switch($type){
    case 'bmp': $img = imagecreatefromwbmp($src); break;
    case 'gif': $img = imagecreatefromgif($src); break;
    case 'jpg': $img = imagecreatefromjpeg($src); break;
    case 'png': $img = imagecreatefrompng($src); break;
    default : return "Unsupported picture type!";
  }

  // resize
  if($crop){
    if($w < $width or $h < $height) return "Picture is too small!";
    $ratio = max($width/$w, $height/$h);
    $h = $height / $ratio;
    $x = ($w - $width / $ratio) / 2;
    $w = $width / $ratio;
  }
  else{
    if($w < $width and $h < $height) return "Picture is too small!";
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
    case 'bmp': imagewbmp($new, $dst); break;
    case 'gif': imagegif($new, $dst); break;
    case 'jpg': imagejpeg($new, $dst); break;
    case 'png': imagepng($new, $dst); break;
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
  $pic_type = strtolower(strrchr($picture['name'],"."));
  $pic_name = "original$pic_type";
  move_uploaded_file($picture['tmp_name'], $pic_name);
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

            $this->format = $ext;
            $this->img_input = ImageCreateFromJPEG($img);
            $this->img_src = $img;
            

        }

        // PNG image
        elseif(is_file($img) &amp;&amp; $ext == "PNG")
        {

            $this->format = $ext;
            $this->img_input = ImageCreateFromPNG($img);
            $this->img_src = $img;

        }

        // GIF image
        elseif(is_file($img) &amp;&amp; $ext == "GIF")
        {

            $this->format = $ext;
            $this->img_input = ImageCreateFromGIF($img);
            $this->img_src = $img;

        }

        // Get dimensions
        $this->x_input = imagesx($this->img_input);
        $this->y_input = imagesy($this->img_input);

    }

    // Set maximum image size (pixels)
    public function set_size($size = 100)
    {

        // Resize
        if($this->x_input > $size &amp;&amp; $this->y_input > $size)
        {

            // Wide
            if($this->x_input >= $this->y_input)
            {

                $this->x_output = $size;
                $this->y_output = ($this->x_output / $this->x_input) * $this->y_input;

            }

            // Tall
            else
            {

                $this->y_output = $size;
                $this->x_output = ($this->y_output / $this->y_input) * $this->x_input;

            }

            // Ready
            $this->resize = TRUE;

        }

        // Don't resize
        else { $this->resize = FALSE; }

    }

    // Set image quality (JPEG only)
    public function set_quality($quality)
    {

        if(is_int($quality))
        {

            $this->quality = $quality;

        }

    }

    // Save image
    public function save_img($path)
    {

        // Resize
        if($this->resize)
        {

            $this->img_output = ImageCreateTrueColor($this->x_output, $this->y_output);
            ImageCopyResampled($this->img_output, $this->img_input, 0, 0, 0, 0, $this->x_output, $this->y_output, $this->x_input, $this->y_input);

        }

        // Save JPEG
        if($this->format == "JPG" OR $this->format == "JPEG")
        {

            if($this->resize) { imageJPEG($this->img_output, $path, $this->quality); }
            else { copy($this->img_src, $path); }

        }

        // Save PNG
        elseif($this->format == "PNG")
        {

            if($this->resize) { imagePNG($this->img_output, $path); }
            else { copy($this->img_src, $path); }

        }

        // Save GIF
        elseif($this->format == "GIF")
        {

            if($this->resize) { imageGIF($this->img_output, $path); }
            else { copy($this->img_src, $path); }

        }

    }

    // Get width
    public function get_width()
    {

        return $this->x_input;

    }

    // Get height
    public function get_height()
    {

        return $this->y_input;

    }

    // Clear image cache
    public function clear_cache()
    {

        @ImageDestroy($this->img_input);
        @ImageDestroy($this->img_output);

    }

}

##### DEMO #####

// Image
$src = "myimage.jpg";

// Begin
$img = new imaging;
$img->set_img($src);
$img->set_quality(80);

// Small thumbnail
$img->set_size(200);
$img->save_img("small_" . $src);

// Baby thumbnail
$img->set_size(50);
$img->save_img("baby_" . $src);

// Finalize
$img->clear_cache();

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopyresampled.php)

**[To root](/README.md)**