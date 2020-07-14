# bbcode_create



For those without the BBCode extension, here&apos;s a relatively elegant function to do the trick. <br>Keep in mind that if you&apos;re using XHTML and one of your users tries to overlap lags &lt;b&gt;Like &lt;i&gt;so&lt;/b&gt;&lt;/i&gt;, it will invalidate your markup. Still working on an expression for this. <br><br>

```
<?php 
    function bb_parse($string) {
        $tags = &apos;b|i|size|color|center|quote|url|img&apos;;
        while (preg_match_all(&apos;`\[(&apos;.$tags.&apos;)=?(.*?)\](.+?)\[/\1\]`&apos;, $string, $matches)) foreach ($matches[0] as $key =&gt; $match) {
            list($tag, $param, $innertext) = array($matches[1][$key], $matches[2][$key], $matches[3][$key]); 
            switch ($tag) { 
                case &apos;b&apos;: $replacement = "&lt;strong&gt;$innertext&lt;/strong&gt;"; break; 
                case &apos;i&apos;: $replacement = "&lt;em&gt;$innertext&lt;/em&gt;"; break; 
                case &apos;size&apos;: $replacement = "&lt;span style=\"font-size: $param;\"&gt;$innertext&lt;/span&gt;"; break; 
                case &apos;color&apos;: $replacement = "&lt;span style=\"color: $param;\"&gt;$innertext&lt;/span&gt;"; break; 
                case &apos;center&apos;: $replacement = "&lt;div class=\"centered\"&gt;$innertext&lt;/div&gt;"; break; 
                case &apos;quote&apos;: $replacement = "&lt;blockquote&gt;$innertext&lt;/blockquote&gt;" . $param? "&lt;cite&gt;$param&lt;/cite&gt;" : &apos;&apos;; break; 
                case &apos;url&apos;: $replacement = &apos;&lt;a href="&apos; . ($param? $param : $innertext) . "\"&gt;$innertext&lt;/a&gt;"; break; 
                case &apos;img&apos;: 
                    list($width, $height) = preg_split(&apos;`[Xx]`&apos;, $param); 
                    $replacement = "&lt;img src=\"$innertext\" " . (is_numeric($width)? "width=\"$width\" " : &apos;&apos;) . (is_numeric($height)? "height=\"$height\" " : &apos;&apos;) . &apos;/&gt;&apos;; 
                break; 
                case &apos;video&apos;: 
                    $videourl = parse_url($innertext); 
                    parse_str($videourl[&apos;query&apos;], $videoquery); 
                    if (strpos($videourl[&apos;host&apos;], &apos;youtube.com&apos;) !== FALSE) $replacement = &apos;&lt;embed src="http://www.youtube.com/v/&apos; . $videoquery[&apos;v&apos;] . &apos;" type="application/x-shockwave-flash" width="425" height="344"&gt;&lt;/embed&gt;&apos;; 
                    if (strpos($videourl[&apos;host&apos;], &apos;google.com&apos;) !== FALSE) $replacement = &apos;&lt;embed src="http://video.google.com/googleplayer.swf?docid=&apos; . $videoquery[&apos;docid&apos;] . &apos;" width="400" height="326" type="application/x-shockwave-flash"&gt;&lt;/embed&gt;&apos;; 
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