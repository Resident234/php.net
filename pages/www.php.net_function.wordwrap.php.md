# wordwrap



Another solution to utf-8 safe wordwrap, unsing regular expressions.<br>Pretty good performance and works in linear time.<br><br>

```
<?php
function utf8_wordwrap($string, $width=75, $break="\n", $cut=false)
{
  if($cut) {
    // Match anything 1 to $width chars long followed by whitespace or EOS,
    // otherwise match anything $width chars long
    $search = &apos;/(.{1,&apos;.$width.&apos;})(?:\s|$)|(.{&apos;.$width.&apos;})/uS&apos;;
    $replace = &apos;$1$2&apos;.$break;
  } else {
    // Anchor the beginning of the pattern with a lookahead
    // to avoid crazy backtracking when words are longer than $width
    $pattern = &apos;/(?=\s)(.{1,&apos;.$width.&apos;})(?:\s|$)/uS&apos;;
    $replace = &apos;$1&apos;.$break;
  }
  return preg_replace($search, $replace, $string);
}
?>
```
<br>Of course don&apos;t forget to use preg_quote on the $width and $break parameters if they come from untrusted input.  

#

For those interested in wrapping text to fit a width in *pixels* (instead of characters), you might find the following function useful; particularly for line-wrapping text over dynamically-generated images.<br><br>If a word is too long to squeeze into the available space, it&apos;ll hyphenate it as needed so it fits the container. This operates recursively, so ridiculously long words or names (e.g., URLs or this guy&apos;s signature - http://en.wikipedia.org/wiki/Wolfe+585,_Senior) will still keep getting broken off after they&apos;ve passed the fourth or fifth lines, or whatever.<br><br>

```
<?php

    /**
     * Wraps a string to a given number of pixels.
     * 
     * This function operates in a similar fashion as PHP&apos;s native wordwrap function; however,
     * it calculates wrapping based on font and point-size, rather than character count. This
     * can generate more even wrapping for sentences with a consider number of thin characters.
     * 
     * @static $mult;
     * @param string $text - Input string.
     * @param float $width - Width, in pixels, of the text&apos;s wrapping area.
     * @param float $size - Size of the font, expressed in pixels.
     * @param string $font - Path to the typeface to measure the text with.
     * @return string The original string with line-breaks manually inserted at detected wrapping points.
     */
    function pixel_word_wrap($text, $width, $size, $font){

        #    Passed a blank value? Bail early.
        if(!$text) return $text;

        #    Check if imagettfbbox is expecting font-size to be declared in points or pixels.
        static $mult;
        $mult    =    $mult ?: version_compare(GD_VERSION, &apos;2.0&apos;, &apos;&gt;=&apos;) ? .75 : 1;

        #    Text already fits the designated space without wrapping.
        $box    =    imagettfbbox($size * $mult, 0, $font, $text);
        if($box[2] - $box[0] / $mult &lt; $width)    return $text;

        #    Start measuring each line of our input and inject line-breaks when overflow&apos;s detected.
        $output        =    &apos;&apos;;
        $length        =    0;

        $words        =    preg_split(&apos;/\b(?=\S)|(?=\s)/&apos;, $text);
        $word_count    =    count($words);
        for($i = 0; $i &lt; $word_count; ++$i){

            #    Newline
            if(PHP_EOL === $words[$i])
                $length    =    0;

            #    Strip any leading tabs.
            if(!$length) $words[$i]    =    preg_replace(&apos;/^\t+/&apos;, &apos;&apos;, $words[$i]);

            $box    =    imagettfbbox($size * $mult, 0, $font, $words[$i]);
            $m        =    $box[2] - $box[0] / $mult;

            #    This is one honkin&apos; long word, so try to hyphenate it.
            if(($diff = $width - $m) &lt;= 0){
                $diff    =    abs($diff);

                #    Figure out which end of the word to start measuring from. Saves a few extra cycles in an already heavy-duty function.
                if($diff - $width &lt;= 0)    for($s = strlen($words[$i]); $s; --$s){
                    $box    =    imagettfbbox($size * $mult, 0, $font, substr($words[$i], 0, $s) . &apos;-&apos;);
                    if($width &gt; ($box[2] - $box[0] / $mult) + $size){
                        $breakpoint    =    $s;
                        break;
                    }
                }

                else{
                    $word_length    =    strlen($words[$i]);
                    for($s = 0; $s &lt; $word_length; ++$s){
                        $box    =    imagettfbbox($size * $mult, 0, $font, substr($words[$i], 0, $s+1) . &apos;-&apos;);
                        if($width &lt; ($box[2] - $box[0] / $mult) + $size){
                            $breakpoint    =    $s;
                            break;
                        }
                    }
                }

                if($breakpoint){
                    $w_l    =    substr($words[$i], 0, $s+1) . &apos;-&apos;;
                    $w_r    =    substr($words[$i],     $s+1);

                    $words[$i]    =    $w_l;
                    array_splice($words, $i+1, 0, $w_r);
                    ++$word_count;
                    $box    =    imagettfbbox($size * $mult, 0, $font, $w_l);
                    $m        =    $box[2] - $box[0] / $mult;
                }
            }

            #    If there&apos;s no more room on the current line to fit the next word, start a new line.
            if($length &gt; 0 &amp;&amp; $length + $m &gt;= $width){
                $output    .=    PHP_EOL;
                $length    =    0;

                #    If the current word is just a space, don&apos;t bother. Skip (saves a weird-looking gap in the text).
                if(&apos; &apos; === $words[$i]) continue;
            }

            #    Write another word and increase the total length of the current line.
            $output    .=    $words[$i];
            $length +=    $m;
        }

        return $output;
    };

?>
```
  

#

If you&apos;d like to break long strings of text but avoid breaking html you may find this useful. It seems to be working for me, hope it works for you. Enjoy. :)<br><br>

```
<?php
    function textWrap($text) {
        $new_text = &apos;&apos;;
        $text_1 = explode(&apos;&gt;&apos;,$text);
        $sizeof = sizeof($text_1);
        for ($i=0; $i&lt;$sizeof; ++$i) {
            $text_2 = explode(&apos;&lt;&apos;,$text_1[$i]);
            if (!empty($text_2[0])) {
                $new_text .= preg_replace(&apos;#([^\n\r .]{25})#i&apos;, &apos;\\1  &apos;, $text_2[0]);
            }
            if (!empty($text_2[1])) {
                $new_text .= &apos;&lt;&apos; . $text_2[1] . &apos;&gt;&apos;;    
            }
        }
        return $new_text;
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.wordwrap.php)

**[To root](/README.md)**