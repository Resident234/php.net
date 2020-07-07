# imageaffine





AFFINE is a geometric transformation operation involving MATRICES, covering both, 2D and 3D environment.
Transformations are often used in linear algebra and computer graphics.

In geometric transformations of images, the pixel coordinate is mapped.

This means that, each pixel is localized by two coordinates, in the rectangular domain of the image.
Without going into more details about pixel mapping, let&apos;s get to what really matters: the AFFINE transformations.
There are several classes of pixel mapping, one is called &quot;affine&quot;.
Affine transformations include: scaling, rotation, shearing and translation.

PHP 5.5.0+, like &quot;libart&quot;, uses ARRAYS of six floating-point elements to do the job.

The AFFINE ARRAY, is defined like, 

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ a0, a1, b0, b1, a2, b2 ];
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; where, 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; a0, a1, b0, b1, a2, b2 are floating-point values.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; represented by equations,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; = a0x + a1y + a2
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = b0x + b1y + b2
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
a ) IDENTITY, no change made in the path of points,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ 1, 0, 0, 1, 0, 0 ];
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; = 1x + 0y + 0 = x
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = 0x + 1y = 0 = y
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
b ) TRANSLATION,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ 1, 0, 0, 1, H, V ];

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; = 1x + 0y + H = x + H
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = 0x + 1y = V = y + V
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; Each point is moved H units horizontaly and V units verticaly.
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
c ) SCALE,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ i, 0, j, 1, 0, 0 ];

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; = Mx + 0y + 0 = Mx
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = 0x + Ny = 0 = Ny
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; Each point will stretch or compress its path, horizontally or vertically, according M and N; negative or positive values.
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
d ) SHEARING, parallel to x axis,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ 1, K, 0, 1, 0, 0 ];

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; = 1x + Ky + 0 = x + Ky
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = 0x + 1y = 0 = y
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; SHEARING, parallel to y axis,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ K, 0, 0, 1, 0, 0 ];

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; = 1x + 0y + 0 = x
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = Kx + 1y = 0 = y + Kx
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
e ) ROTATION, clockwise,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ cos &#xD8;, sin &#xD8;, -sin &#xD8;, cos &#xD8;, 0, 0 ];

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; =&#xA0; x cos &#xD8; + y sin &#xD8; + 0 = x cos &#xD8; + y sin &#xD8;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = -x sin &#xD8; + y cos &#xD8; = 0 = y cos &#xD8; - x sin &#xD8;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; ROTATION, conterclockwise,

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $affine = [ cos &#xD8;, -sin &#xD8;, sin &#xD8;, cos &#xD8;, 0, 0 ];

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; equations remapping,
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; x&apos; =&#xA0; x cos &#xD8; - y sin &#xD8; + 0 = x cos &#xD8; - y sin &#xD8;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; y&apos; = x sin &#xD8; + y cos &#xD8; = 0 = y sin &#xD8; - x cos &#xD8;

  

#

[Official documentation page](https://www.php.net/manual/en/function.imageaffine.php)

**[To root](/README.md)**