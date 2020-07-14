# get_meta_tags



This regex gets meta tags independent of sequence by capturing inside a lookahead.<br>Further uses the branch reset feature for different quote styles of values.<br>The pattern can be tested here: https://regex101.com/r/oE4oU9/1<br><br>&lt;?PHP<br><br>function getMetaTags($str)<br>{<br>  $pattern = &apos;<br>  ~&lt;\s*meta\s<br><br>  # using lookahead to capture type to $1<br>    (?=[^&gt;]*?<br>    \b(?:name|property|http-equiv)\s*=\s*<br>    (?|"\s*([^"]*?)\s*"|\&apos;\s*([^\&apos;]*?)\s*\&apos;|<br>    ([^"\&apos;&gt;]*?)(?=\s*/?\s*&gt;|\s\w+\s*=))<br>  )<br><br>  # capture content to $2<br>  [^&gt;]*?\bcontent\s*=\s*<br>    (?|"\s*([^"]*?)\s*"|\&apos;\s*([^\&apos;]*?)\s*\&apos;|<br>    ([^"\&apos;&gt;]*?)(?=\s*/?\s*&gt;|\s\w+\s*=))<br>  [^&gt;]*&gt;<br><br>  ~ix&apos;;<br>  <br>  if(preg_match_all($pattern, $str, $out))<br>    return array_combine($out[1], $out[2]);<br>  return array();<br>}<br><br>// usage<br>$meta_tags = getMetaTags($str);<br><br>?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-meta-tags.php)

**[To root](/README.md)**