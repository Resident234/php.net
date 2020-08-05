# get_meta_tags



This regex gets meta tags independent of sequence by capturing inside a lookahead.<br>Further uses the branch reset feature for different quote styles of values.<br>The pattern can be tested here: https://regex101.com/r/oE4oU9/1<br><br>

```
<?php

function getMetaTags($str)
{
  $pattern = '
  ~<\s*meta\s

  # using lookahead to capture type to $1
    (?=[^>.*?
    \b(?:name|property|http-equiv)\s*=\s*
    (?|"\s*([^".*?)\s*"|\'\s*([^\'.*?)\s*\'|
    ([^"\'>.*?)(?=\s*/?\s*>|\s\w+\s*=))
  )

  # capture content to $2
  [^>.*?\bcontent\s*=\s*
    (?|"\s*([^".*?)\s*"|\'\s*([^\'.*?)\s*\'|
    ([^"\'>.*?)(?=\s*/?\s*>|\s\w+\s*=))
  [^>]*>

  ~ix';
  
  if(preg_match_all($pattern, $str, $out))
    return array_combine($out[1], $out[2]);
  return array();
}

// usage
$meta_tags = getMetaTags($str);

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.get-meta-tags.php)

**[To root](/README.md)**