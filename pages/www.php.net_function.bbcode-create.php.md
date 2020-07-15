# bbcode_create



For those without the BBCode extension, here&apos;s a relatively elegant function to do the trick. <br>Keep in mind that if you&apos;re using XHTML and one of your users tries to overlap lags &lt;b&gt;Like &lt;i&gt;so&lt;/b&gt;&lt;/i&gt;, it will invalidate your markup. Still working on an expression for this. <br><br>

```
<?php 
    function bb_parse($string) {
        $tags = 'b|i|size|color|center|quote|url|img';
        while (preg_match_all('`\[('.$tags.')=?(.*?)\](.+?)\[/\1\]`', $string, $matches)) foreach ($matches[0] as $key => $match) {
            list($tag, $param, $innertext) = array($matches[1][$key], $matches[2][$key], $matches[3][$key]); 
            switch ($tag) { 
                case 'b': $replacement = "&lt;strong&gt;$innertext&lt;/strong&gt;"; break; 
                case 'i': $replacement = "&lt;em&gt;$innertext&lt;/em&gt;"; break; 
                case 'size': $replacement = "&lt;span style=\"font-size: $param;\"&gt;$innertext&lt;/span&gt;"; break; 
                case 'color': $replacement = "&lt;span style=\"color: $param;\"&gt;$innertext&lt;/span&gt;"; break; 
                case 'center': $replacement = "&lt;div class=\"centered\"&gt;$innertext&lt;/div&gt;"; break; 
                case 'quote': $replacement = "&lt;blockquote&gt;$innertext&lt;/blockquote&gt;" . $param? "&lt;cite&gt;$param&lt;/cite&gt;" : ''; break; 
                case 'url': $replacement = '&lt;a href="' . ($param? $param : $innertext) . "\"&gt;$innertext&lt;/a&gt;"; break; 
                case 'img': 
                    list($width, $height) = preg_split('`[Xx]`', $param); 
                    $replacement = "&lt;img src=\"$innertext\" " . (is_numeric($width)? "width=\"$width\" " : '') . (is_numeric($height)? "height=\"$height\" " : '') . '/&gt;'; 
                break; 
                case 'video': 
                    $videourl = parse_url($innertext); 
                    parse_str($videourl['query'], $videoquery); 
                    if (strpos($videourl['host'], 'youtube.com') !== FALSE) $replacement = '&lt;embed src="http://www.youtube.com/v/' . $videoquery['v'] . '" type="application/x-shockwave-flash" width="425" height="344"&gt;&lt;/embed&gt;'; 
                    if (strpos($videourl['host'], 'google.com') !== FALSE) $replacement = '&lt;embed src="http://video.google.com/googleplayer.swf?docid=' . $videoquery['docid'] . '" width="400" height="326" type="application/x-shockwave-flash"&gt;&lt;/embed&gt;'; 
                break; 
            } 
            $string = str_replace($match, $replacement, $string); 
        } 
        return $string; 
    } 
?>
```
<br><br>[EDIT BY danbrown AT php DOT net: Contains a bugfix provided by (ramonvandam AT gmail DOT com) on 04-SEP-09 to address an improperly-defined parameter.  Also contains a bugfix provided by (pompei2 AT gmail DOT com) on 15-FEB-10 to address improperly-closed tags.  Plus, contains another bugfix provided by (angad AT wootify DOT com) on 18-JUL-2011 to fix an issue where unsupported tags provided to the function could cause the script to time out.]  

#

[Official documentation page](https://www.php.net/manual/en/function.bbcode-create.php)

**[To root](/README.md)**