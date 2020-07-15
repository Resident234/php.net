# urldecode



When the client send Get data, utf-8 character encoding have a tiny problem with the urlencode.<br>Consider the "&#xBA;" character. <br>Some clients can send (as example)<br>foo.php?myvar=%BA<br>and another clients send<br>foo.php?myvar=%C2%BA (The "right" url encoding)<br><br>in this scenary, you assign the value into variable $x<br><br>

```
<?php
$x = $_GET['myvar'];
?>
```


$x store: in the first case "&#xFFFD;" (bad) and in the second case "&#xBA;" (good)

To fix that, you can use this function:



```
<?php
function to_utf8( $string ) {
// From http://w3.org/International/questions/qa-forms-utf-8.html
    if ( preg_match('%^(?:
      [\x09\x0A\x0D\x20-\x7E]            # ASCII
    | [\xC2-\xDF][\x80-\xBF]             # non-overlong 2-byte
    | \xE0[\xA0-\xBF][\x80-\xBF]         # excluding overlongs
    | [\xE1-\xEC\xEE\xEF][\x80-\xBF]{2}  # straight 3-byte
    | \xED[\x80-\x9F][\x80-\xBF]         # excluding surrogates
    | \xF0[\x90-\xBF][\x80-\xBF]{2}      # planes 1-3
    | [\xF1-\xF3][\x80-\xBF]{3}          # planes 4-15
    | \xF4[\x80-\x8F][\x80-\xBF]{2}      # plane 16
)*$%xs', $string) ) {
        return $string;
    } else {
        return iconv( 'CP1252', 'UTF-8', $string);
    }
}
?>
```


and assign in this way:



```
<?php
$x = to_utf8( $_GET['myvar'] );
?>
```
<br><br>$x store: in the first case "&#xBA;" (good) and in the second case "&#xBA;" (good)<br><br>Solve a lot of i18n problems.<br><br>Please fix the auto-urldecode of $_GET var in the next PHP version.<br><br>Bye.<br><br>Alejandro Salamanca  

#

If you are escaping strings in javascript and want to decode them in PHP with urldecode (or want PHP to decode them automatically when you&apos;re putting them in the query string or post request), you should use the javascript function encodeURIComponent() instead of escape(). Then you won&apos;t need any of the fancy custom utf_urldecode functions from the previous comments.  

#

[Official documentation page](https://www.php.net/manual/en/function.urldecode.php)

**[To root](/README.md)**