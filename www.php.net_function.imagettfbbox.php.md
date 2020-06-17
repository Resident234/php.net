# imagettfbbox




<div class="phpcode"><span class="html">
I wrote a simple function that calculates the *exact* bounding box (single pixel precision).
<br>The function returns an associative array with these keys:
<br>left, top:&#xA0; coordinates you will pass to imagettftext 
<br>width, height: dimension of the image you have to create
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">calculateTextBox</span><span class="keyword">(</span><span class="default">$font_size</span><span class="keyword">, </span><span class="default">$font_angle</span><span class="keyword">, </span><span class="default">$font_file</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">) {
<br>&#xA0; </span><span class="default">$box&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">imagettfbbox</span><span class="keyword">(</span><span class="default">$font_size</span><span class="keyword">, </span><span class="default">$font_angle</span><span class="keyword">, </span><span class="default">$font_file</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);
<br>&#xA0; if( !</span><span class="default">$box </span><span class="keyword">)
<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$min_x </span><span class="keyword">= </span><span class="default">min</span><span class="keyword">( array(</span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">6</span><span class="keyword">]) );
<br>&#xA0; </span><span class="default">$max_x </span><span class="keyword">= </span><span class="default">max</span><span class="keyword">( array(</span><span class="default">$box</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">6</span><span class="keyword">]) );
<br>&#xA0; </span><span class="default">$min_y </span><span class="keyword">= </span><span class="default">min</span><span class="keyword">( array(</span><span class="default">$box</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">7</span><span class="keyword">]) );
<br>&#xA0; </span><span class="default">$max_y </span><span class="keyword">= </span><span class="default">max</span><span class="keyword">( array(</span><span class="default">$box</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">], </span><span class="default">$box</span><span class="keyword">[</span><span class="default">7</span><span class="keyword">]) );
<br>&#xA0; </span><span class="default">$width&#xA0; </span><span class="keyword">= ( </span><span class="default">$max_x </span><span class="keyword">- </span><span class="default">$min_x </span><span class="keyword">);
<br>&#xA0; </span><span class="default">$height </span><span class="keyword">= ( </span><span class="default">$max_y </span><span class="keyword">- </span><span class="default">$min_y </span><span class="keyword">);
<br>&#xA0; </span><span class="default">$left&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">abs</span><span class="keyword">( </span><span class="default">$min_x </span><span class="keyword">) + </span><span class="default">$width</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$top&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">abs</span><span class="keyword">( </span><span class="default">$min_y </span><span class="keyword">) + </span><span class="default">$height</span><span class="keyword">;
<br>&#xA0; </span><span class="comment">// to calculate the exact bounding box i write the text in a large image
<br>&#xA0; </span><span class="default">$img&#xA0; &#xA0;&#xA0; </span><span class="keyword">= @</span><span class="default">imagecreatetruecolor</span><span class="keyword">( </span><span class="default">$width </span><span class="keyword">&lt;&lt; </span><span class="default">2</span><span class="keyword">, </span><span class="default">$height </span><span class="keyword">&lt;&lt; </span><span class="default">2 </span><span class="keyword">);
<br>&#xA0; </span><span class="default">$white&#xA0;&#xA0; </span><span class="keyword">=&#xA0; </span><span class="default">imagecolorallocate</span><span class="keyword">( </span><span class="default">$img</span><span class="keyword">, </span><span class="default">255</span><span class="keyword">, </span><span class="default">255</span><span class="keyword">, </span><span class="default">255 </span><span class="keyword">);
<br>&#xA0; </span><span class="default">$black&#xA0;&#xA0; </span><span class="keyword">=&#xA0; </span><span class="default">imagecolorallocate</span><span class="keyword">( </span><span class="default">$img</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0 </span><span class="keyword">);
<br>&#xA0; </span><span class="default">imagefilledrectangle</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">imagesx</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">), </span><span class="default">imagesy</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">), </span><span class="default">$black</span><span class="keyword">);
<br>&#xA0; </span><span class="comment">// for sure the text is completely in the image!
<br>&#xA0; </span><span class="default">imagettftext</span><span class="keyword">( </span><span class="default">$img</span><span class="keyword">, </span><span class="default">$font_size</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$font_angle</span><span class="keyword">, </span><span class="default">$left</span><span class="keyword">, </span><span class="default">$top</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$white</span><span class="keyword">, </span><span class="default">$font_file</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);
<br>&#xA0; </span><span class="comment">// start scanning (0=&gt; black =&gt; empty)
<br>&#xA0; </span><span class="default">$rleft&#xA0; </span><span class="keyword">= </span><span class="default">$w4 </span><span class="keyword">= </span><span class="default">$width</span><span class="keyword">&lt;&lt;</span><span class="default">2</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$rright </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$rbottom&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; </span><span class="default">$rtop </span><span class="keyword">= </span><span class="default">$h4 </span><span class="keyword">= </span><span class="default">$height</span><span class="keyword">&lt;&lt;</span><span class="default">2</span><span class="keyword">;
<br>&#xA0; for( </span><span class="default">$x </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$x </span><span class="keyword">&lt; </span><span class="default">$w4</span><span class="keyword">; </span><span class="default">$x</span><span class="keyword">++ )
<br>&#xA0; &#xA0; for( </span><span class="default">$y </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$y </span><span class="keyword">&lt; </span><span class="default">$h4</span><span class="keyword">; </span><span class="default">$y</span><span class="keyword">++ )
<br>&#xA0; &#xA0; &#xA0; if( </span><span class="default">imagecolorat</span><span class="keyword">( </span><span class="default">$img</span><span class="keyword">, </span><span class="default">$x</span><span class="keyword">, </span><span class="default">$y </span><span class="keyword">) ){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rleft&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">min</span><span class="keyword">( </span><span class="default">$rleft</span><span class="keyword">, </span><span class="default">$x </span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rright&#xA0; </span><span class="keyword">= </span><span class="default">max</span><span class="keyword">( </span><span class="default">$rright</span><span class="keyword">, </span><span class="default">$x </span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rtop&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">min</span><span class="keyword">( </span><span class="default">$rtop</span><span class="keyword">, </span><span class="default">$y </span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rbottom </span><span class="keyword">= </span><span class="default">max</span><span class="keyword">( </span><span class="default">$rbottom</span><span class="keyword">, </span><span class="default">$y </span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; </span><span class="comment">// destroy img and serve the result
<br>&#xA0; </span><span class="default">imagedestroy</span><span class="keyword">( </span><span class="default">$img </span><span class="keyword">);
<br>&#xA0; return array( </span><span class="string">&quot;left&quot;&#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">$left </span><span class="keyword">- </span><span class="default">$rleft</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;top&quot;&#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">$top&#xA0; </span><span class="keyword">- </span><span class="default">$rtop</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;width&quot;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">$rright </span><span class="keyword">- </span><span class="default">$rleft </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;height&quot; </span><span class="keyword">=&gt; </span><span class="default">$rbottom </span><span class="keyword">- </span><span class="default">$rtop </span><span class="keyword">+ </span><span class="default">1 </span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that as imageTTFBbox and imageTTFText functions return an array of coordinates which could be negative numbers care must be taken with height and width calculations.<br><br>The rigth way to do that is to use the abs() function:<br><br>for an horizontal text:<br><br>$box = @imageTTFBbox($size,0,$font,$text);<br>$width = abs($box[4] - $box[0]);<br>$height = abs($box[5] - $box[1]);<br><br>Then to center your text at ($x,$y) position the code should be like that:<br><br>$x -= $width/2;<br>$y += $heigth/2;<br><br>imageTTFText($img,$size,0,$x,$y,$color,$font,$text);<br><br>this because (0,0) page origin is topleft page corner and (0,0) text origin is lower-left readable text corner.<br><br>Hope this help.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagettfbbox.php)

**[To root](/README.md)**