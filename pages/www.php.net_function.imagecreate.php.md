# imagecreate





to create an image from a BMP file, I made this function, that return a resource like the others ImageCreateFrom function:



```
<?php
/*********************************************/
/* Fonction: ImageCreateFromBMP&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; */
/* Author:&#xA0;&#xA0; DHKold&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; */
/* Contact:&#xA0; admin@dhkold.com&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; */
/* Date:&#xA0; &#xA0;&#xA0; The 15th of June 2005&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; */
/* Version:&#xA0; 2.0B&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; */
/*********************************************/

function ImageCreateFromBMP($filename)
{
 //Ouverture du fichier en mode binaire
&#xA0;&#xA0; if (! $f1 = fopen($filename,&quot;rb&quot;)) return FALSE;

 //1 : Chargement des ent&#xFFFD;tes FICHIER
&#xA0;&#xA0; $FILE = unpack(&quot;vfile_type/Vfile_size/Vreserved/Vbitmap_offset&quot;, fread($f1,14));
&#xA0;&#xA0; if ($FILE[&apos;file_type&apos;] != 19778) return FALSE;

 //2 : Chargement des ent&#xFFFD;tes BMP
&#xA0;&#xA0; $BMP = unpack(&apos;Vheader_size/Vwidth/Vheight/vplanes/vbits_per_pixel&apos;.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;/Vcompression/Vsize_bitmap/Vhoriz_resolution&apos;.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;/Vvert_resolution/Vcolors_used/Vcolors_important&apos;, fread($f1,40));
&#xA0;&#xA0; $BMP[&apos;colors&apos;] = pow(2,$BMP[&apos;bits_per_pixel&apos;]);
&#xA0;&#xA0; if ($BMP[&apos;size_bitmap&apos;] == 0) $BMP[&apos;size_bitmap&apos;] = $FILE[&apos;file_size&apos;] - $FILE[&apos;bitmap_offset&apos;];
&#xA0;&#xA0; $BMP[&apos;bytes_per_pixel&apos;] = $BMP[&apos;bits_per_pixel&apos;]/8;
&#xA0;&#xA0; $BMP[&apos;bytes_per_pixel2&apos;] = ceil($BMP[&apos;bytes_per_pixel&apos;]);
&#xA0;&#xA0; $BMP[&apos;decal&apos;] = ($BMP[&apos;width&apos;]*$BMP[&apos;bytes_per_pixel&apos;]/4);
&#xA0;&#xA0; $BMP[&apos;decal&apos;] -= floor($BMP[&apos;width&apos;]*$BMP[&apos;bytes_per_pixel&apos;]/4);
&#xA0;&#xA0; $BMP[&apos;decal&apos;] = 4-(4*$BMP[&apos;decal&apos;]);
&#xA0;&#xA0; if ($BMP[&apos;decal&apos;] == 4) $BMP[&apos;decal&apos;] = 0;

 //3 : Chargement des couleurs de la palette
&#xA0;&#xA0; $PALETTE = array();
&#xA0;&#xA0; if ($BMP[&apos;colors&apos;] &lt; 16777216)
&#xA0;&#xA0; {
&#xA0; &#xA0; $PALETTE = unpack(&apos;V&apos;.$BMP[&apos;colors&apos;], fread($f1,$BMP[&apos;colors&apos;]*4));
&#xA0;&#xA0; }

 //4 : Cr&#xFFFD;ation de l&apos;image
&#xA0;&#xA0; $IMG = fread($f1,$BMP[&apos;size_bitmap&apos;]);
&#xA0;&#xA0; $VIDE = chr(0);

&#xA0;&#xA0; $res = imagecreatetruecolor($BMP[&apos;width&apos;],$BMP[&apos;height&apos;]);
&#xA0;&#xA0; $P = 0;
&#xA0;&#xA0; $Y = $BMP[&apos;height&apos;]-1;
&#xA0;&#xA0; while ($Y &gt;= 0)
&#xA0;&#xA0; {
&#xA0; &#xA0; $X=0;
&#xA0; &#xA0; while ($X &lt; $BMP[&apos;width&apos;])
&#xA0; &#xA0; {
&#xA0; &#xA0;&#xA0; if ($BMP[&apos;bits_per_pixel&apos;] == 24)
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR = unpack(&quot;V&quot;,substr($IMG,$P,3).$VIDE);
&#xA0; &#xA0;&#xA0; elseif ($BMP[&apos;bits_per_pixel&apos;] == 16)
&#xA0; &#xA0;&#xA0; {&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR = unpack(&quot;n&quot;,substr($IMG,$P,2));
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR[1] = $PALETTE[$COLOR[1]+1];
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; elseif ($BMP[&apos;bits_per_pixel&apos;] == 8)
&#xA0; &#xA0;&#xA0; {&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR = unpack(&quot;n&quot;,$VIDE.substr($IMG,$P,1));
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR[1] = $PALETTE[$COLOR[1]+1];
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; elseif ($BMP[&apos;bits_per_pixel&apos;] == 4)
&#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR = unpack(&quot;n&quot;,$VIDE.substr($IMG,floor($P),1));
&#xA0; &#xA0; &#xA0; &#xA0; if (($P*2)%2 == 0) $COLOR[1] = ($COLOR[1] &gt;&gt; 4) ; else $COLOR[1] = ($COLOR[1] &amp; 0x0F);
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR[1] = $PALETTE[$COLOR[1]+1];
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; elseif ($BMP[&apos;bits_per_pixel&apos;] == 1)
&#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR = unpack(&quot;n&quot;,$VIDE.substr($IMG,floor($P),1));
&#xA0; &#xA0; &#xA0; &#xA0; if&#xA0; &#xA0;&#xA0; (($P*8)%8 == 0) $COLOR[1] =&#xA0; $COLOR[1]&#xA0; &#xA0; &#xA0; &#xA0; &gt;&gt;7;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 1) $COLOR[1] = ($COLOR[1] &amp; 0x40)&gt;&gt;6;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 2) $COLOR[1] = ($COLOR[1] &amp; 0x20)&gt;&gt;5;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 3) $COLOR[1] = ($COLOR[1] &amp; 0x10)&gt;&gt;4;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 4) $COLOR[1] = ($COLOR[1] &amp; 0x8)&gt;&gt;3;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 5) $COLOR[1] = ($COLOR[1] &amp; 0x4)&gt;&gt;2;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 6) $COLOR[1] = ($COLOR[1] &amp; 0x2)&gt;&gt;1;
&#xA0; &#xA0; &#xA0; &#xA0; elseif (($P*8)%8 == 7) $COLOR[1] = ($COLOR[1] &amp; 0x1);
&#xA0; &#xA0; &#xA0; &#xA0; $COLOR[1] = $PALETTE[$COLOR[1]+1];
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; return FALSE;
&#xA0; &#xA0;&#xA0; imagesetpixel($res,$X,$Y,$COLOR[1]);
&#xA0; &#xA0;&#xA0; $X++;
&#xA0; &#xA0;&#xA0; $P += $BMP[&apos;bytes_per_pixel&apos;];
&#xA0; &#xA0; }
&#xA0; &#xA0; $Y--;
&#xA0; &#xA0; $P+=$BMP[&apos;decal&apos;];
&#xA0;&#xA0; }

 //Fermeture du fichier
&#xA0;&#xA0; fclose($f1);

 return $res;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreate.php)

**[To root](/README.md)**