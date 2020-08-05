# bbcode_create



For those without the BBCode extension, here&apos;s a relatively elegant function to do the trick. <br>Keep in mind that if you&apos;re using XHTML and one of your users tries to overlap lags &lt;b&gt;Like &lt;i&gt;so&lt;/b&gt;&lt;/i&gt;, it will invalidate your markup. Still working on an expression for this. <br><br>

```
<?php 
    function bb_parse($string) {
        $tags = 'b|i|size|color|center|quote|url|img';
        while (preg_match_all('`\[('.$tags.')=?(.*?)\](.+?)\[/\1\]`', $string, $matches)) foreach ($matches[0] as $key => $match) {
            list($tag, $param, $innertext) = array($matches[1][$key], $matches[2][$key], $matches[3][$key]); 
            switch ($tag) { 
                case 'b': $replacement = "<strong>$innertext</strong>"; break; 
                case 'i': $replacement = "<em>$innertext</em>"; break; 
                case 'size': $replacement = "<span style=\"font-size: $param;\">$innertext</span>"; break; 
                case 'color': $replacement = "<span style=\"color: $param;\">$innertext</span>"; break; 
                case 'center': $replacement = "<div class=\"centered\">$innertext</div>"; break; 
                case 'quote': $replacement = "<blockquote>$innertext</blockquote>" . $param? "<cite>$param</cite>" : ''; break; 
                case 'url': $replacement = '<a href="' . ($param? $param : $innertext) . "\">$innertext</a>"; break; 
                case 'img': 
                    list($width, $height) = preg_split('`[Xx]`', $param); 
                    $replacement = "<img src=\"$innertext\" " . (is_numeric($width)? "width=\"$width\" " : '') . (is_numeric($height)? "height=\"$height\" " : '') . '/>'; 
                break; 
                case 'video': 
                    $videourl = parse_url($innertext); 
                    parse_str($videourl['query'], $videoquery); 
                    if (strpos($videourl['host'], 'youtube.com') !== FALSE) $replacement = '<embed src="http://www.youtube.com/v/' . $videoquery['v'] . '" type="application/x-shockwave-flash" width="425" height="344"></embed>'; 
                    if (strpos($videourl['host'], 'google.com') !== FALSE) $replacement = '<embed src="http://video.google.com/googleplayer.swf?docid=' . $videoquery['docid'] . '" width="400" height="326" type="application/x-shockwave-flash"></embed>'; 
                break; 
            } 
            $string = str_replace($match, $replacement, $string); 
        } 
        return $string; 
    } 
?>
```
<br><br>[EDIT BY danbrown AT php DOT net: Contains a bugfix provided by (ramonvandam AT gmail DOT com) on 04-SEP-09 to address an improperly-defined parameter.  Also contains a bugfix provided by (pompei2 AT gmail DOT com) on 15-FEB-10 to address improperly-closed tags.  Plus, contains another bugfix provided by (angad AT wootify DOT com) on 18-JUL-2011 to fix an issue where unsupported tags provided to the function could cause the script to time out.]  

---

[Official documentation page](https://www.php.net/manual/en/function.bbcode-create.php)

**[To root](/README.md)**