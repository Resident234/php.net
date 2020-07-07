# imagecopyresampled





FOUR RECTANGLES

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $src_image&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $dst_image
+------------+---------------------------------+&#xA0;&#xA0; +------------+--------------------+
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $dst_y&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $src_y&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; +-- $dst_x --+----$dst_width----+ |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; Resampled&#xA0; &#xA0;&#xA0; | |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | |
+-- $src_x --+------ $src_width ------+&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0;&#xA0; $dst_height&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +------------------+ |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; Sample&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0; &#xA0; &#xA0;&#xA0; $src_height&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |&#xA0;&#xA0; +---------------------------------+
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +------------------------+&#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
|&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
+----------------------------------------------+

  

#



Here is my ultimate image resizer that preserves transparency for gif&apos;s and png&apos;s and has an option to crop images to fixed dimensions (preserves image proportions by default)



```
<?php
function image_resize($src, $dst, $width, $height, $crop=0){

&#xA0; if(!list($w, $h) = getimagesize($src)) return &quot;Unsupported picture type!&quot;;

&#xA0; $type = strtolower(substr(strrchr($src,&quot;.&quot;),1));
&#xA0; if($type == &apos;jpeg&apos;) $type = &apos;jpg&apos;;
&#xA0; switch($type){
&#xA0; &#xA0; case &apos;bmp&apos;: $img = imagecreatefromwbmp($src); break;
&#xA0; &#xA0; case &apos;gif&apos;: $img = imagecreatefromgif($src); break;
&#xA0; &#xA0; case &apos;jpg&apos;: $img = imagecreatefromjpeg($src); break;
&#xA0; &#xA0; case &apos;png&apos;: $img = imagecreatefrompng($src); break;
&#xA0; &#xA0; default : return &quot;Unsupported picture type!&quot;;
&#xA0; }

&#xA0; // resize
&#xA0; if($crop){
&#xA0; &#xA0; if($w &lt; $width or $h &lt; $height) return &quot;Picture is too small!&quot;;
&#xA0; &#xA0; $ratio = max($width/$w, $height/$h);
&#xA0; &#xA0; $h = $height / $ratio;
&#xA0; &#xA0; $x = ($w - $width / $ratio) / 2;
&#xA0; &#xA0; $w = $width / $ratio;
&#xA0; }
&#xA0; else{
&#xA0; &#xA0; if($w &lt; $width and $h &lt; $height) return &quot;Picture is too small!&quot;;
&#xA0; &#xA0; $ratio = min($width/$w, $height/$h);
&#xA0; &#xA0; $width = $w * $ratio;
&#xA0; &#xA0; $height = $h * $ratio;
&#xA0; &#xA0; $x = 0;
&#xA0; }

&#xA0; $new = imagecreatetruecolor($width, $height);

&#xA0; // preserve transparency
&#xA0; if($type == &quot;gif&quot; or $type == &quot;png&quot;){
&#xA0; &#xA0; imagecolortransparent($new, imagecolorallocatealpha($new, 0, 0, 0, 127));
&#xA0; &#xA0; imagealphablending($new, false);
&#xA0; &#xA0; imagesavealpha($new, true);
&#xA0; }

&#xA0; imagecopyresampled($new, $img, 0, 0, $x, 0, $width, $height, $w, $h);

&#xA0; switch($type){
&#xA0; &#xA0; case &apos;bmp&apos;: imagewbmp($new, $dst); break;
&#xA0; &#xA0; case &apos;gif&apos;: imagegif($new, $dst); break;
&#xA0; &#xA0; case &apos;jpg&apos;: imagejpeg($new, $dst); break;
&#xA0; &#xA0; case &apos;png&apos;: imagepng($new, $dst); break;
&#xA0; }
&#xA0; return true;
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
&#xA0; $pic_type = strtolower(strrchr($picture[&apos;name&apos;],&quot;.&quot;));
&#xA0; $pic_name = &quot;original$pic_type&quot;;
&#xA0; move_uploaded_file($picture[&apos;tmp_name&apos;], $pic_name);
&#xA0; if (true !== ($pic_error = @image_resize($pic_name, &quot;100x100$pic_type&quot;, 100, 100, 1))) {
&#xA0; &#xA0; echo $pic_error;
&#xA0; &#xA0; unlink($pic_name);
&#xA0; }
&#xA0; else echo &quot;OK!&quot;;
?>
```


Cheers!

  

#



I&apos;ve created a PHP5 image resize class, using ImageCopyResampled, that someone might find useful, with support for JPEG, PNG, and GIF formats. It retains the original image&apos;s aspect ratio when resizing, and doesn&apos;t resize or resample if the original width and height is smaller then the desired resize.



```
<?php

// Imaging
class imaging
{

&#xA0; &#xA0; // Variables
&#xA0; &#xA0; private $img_input;
&#xA0; &#xA0; private $img_output;
&#xA0; &#xA0; private $img_src;
&#xA0; &#xA0; private $format;
&#xA0; &#xA0; private $quality = 80;
&#xA0; &#xA0; private $x_input;
&#xA0; &#xA0; private $y_input;
&#xA0; &#xA0; private $x_output;
&#xA0; &#xA0; private $y_output;
&#xA0; &#xA0; private $resize;

&#xA0; &#xA0; // Set image
&#xA0; &#xA0; public function set_img($img)
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; // Find format
&#xA0; &#xA0; &#xA0; &#xA0; $ext = strtoupper(pathinfo($img, PATHINFO_EXTENSION));

&#xA0; &#xA0; &#xA0; &#xA0; // JPEG image
&#xA0; &#xA0; &#xA0; &#xA0; if(is_file($img) &amp;&amp; ($ext == &quot;JPG&quot; OR $ext == &quot;JPEG&quot;))
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;format = $ext;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_input = ImageCreateFromJPEG($img);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_src = $img;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // PNG image
&#xA0; &#xA0; &#xA0; &#xA0; elseif(is_file($img) &amp;&amp; $ext == &quot;PNG&quot;)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;format = $ext;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_input = ImageCreateFromPNG($img);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_src = $img;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // GIF image
&#xA0; &#xA0; &#xA0; &#xA0; elseif(is_file($img) &amp;&amp; $ext == &quot;GIF&quot;)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;format = $ext;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_input = ImageCreateFromGIF($img);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_src = $img;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Get dimensions
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;x_input = imagesx($this-&gt;img_input);
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;y_input = imagesy($this-&gt;img_input);

&#xA0; &#xA0; }

&#xA0; &#xA0; // Set maximum image size (pixels)
&#xA0; &#xA0; public function set_size($size = 100)
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; // Resize
&#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;x_input &gt; $size &amp;&amp; $this-&gt;y_input &gt; $size)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Wide
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;x_input &gt;= $this-&gt;y_input)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;x_output = $size;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;y_output = ($this-&gt;x_output / $this-&gt;x_input) * $this-&gt;y_input;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Tall
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;y_output = $size;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;x_output = ($this-&gt;y_output / $this-&gt;y_input) * $this-&gt;x_input;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Ready
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;resize = TRUE;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Don&apos;t resize
&#xA0; &#xA0; &#xA0; &#xA0; else { $this-&gt;resize = FALSE; }

&#xA0; &#xA0; }

&#xA0; &#xA0; // Set image quality (JPEG only)
&#xA0; &#xA0; public function set_quality($quality)
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; if(is_int($quality))
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;quality = $quality;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; // Save image
&#xA0; &#xA0; public function save_img($path)
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; // Resize
&#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;resize)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;img_output = ImageCreateTrueColor($this-&gt;x_output, $this-&gt;y_output);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ImageCopyResampled($this-&gt;img_output, $this-&gt;img_input, 0, 0, 0, 0, $this-&gt;x_output, $this-&gt;y_output, $this-&gt;x_input, $this-&gt;y_input);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Save JPEG
&#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;format == &quot;JPG&quot; OR $this-&gt;format == &quot;JPEG&quot;)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;resize) { imageJPEG($this-&gt;img_output, $path, $this-&gt;quality); }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else { copy($this-&gt;img_src, $path); }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Save PNG
&#xA0; &#xA0; &#xA0; &#xA0; elseif($this-&gt;format == &quot;PNG&quot;)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;resize) { imagePNG($this-&gt;img_output, $path); }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else { copy($this-&gt;img_src, $path); }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Save GIF
&#xA0; &#xA0; &#xA0; &#xA0; elseif($this-&gt;format == &quot;GIF&quot;)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;resize) { imageGIF($this-&gt;img_output, $path); }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else { copy($this-&gt;img_src, $path); }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; // Get width
&#xA0; &#xA0; public function get_width()
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;x_input;

&#xA0; &#xA0; }

&#xA0; &#xA0; // Get height
&#xA0; &#xA0; public function get_height()
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;y_input;

&#xA0; &#xA0; }

&#xA0; &#xA0; // Clear image cache
&#xA0; &#xA0; public function clear_cache()
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; @ImageDestroy($this-&gt;img_input);
&#xA0; &#xA0; &#xA0; &#xA0; @ImageDestroy($this-&gt;img_output);

&#xA0; &#xA0; }

}

##### DEMO #####

// Image
$src = &quot;myimage.jpg&quot;;

// Begin
$img = new imaging;
$img-&gt;set_img($src);
$img-&gt;set_quality(80);

// Small thumbnail
$img-&gt;set_size(200);
$img-&gt;save_img(&quot;small_&quot; . $src);

// Baby thumbnail
$img-&gt;set_size(50);
$img-&gt;save_img(&quot;baby_&quot; . $src);

// Finalize
$img-&gt;clear_cache();

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopyresampled.php)

**[To root](/README.md)**