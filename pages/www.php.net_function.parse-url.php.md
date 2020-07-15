# parse_url



[If you haven&apos;t yet] been able to find a simple conversion back to string from a parsed url, here&apos;s an example:<br><br>

```
<?php

$url = 'http://usr:pss@example.com:81/mypath/myfile.html?a=b&amp;b[]=2&amp;b[]=3#myfragment';
if ($url === unparse_url(parse_url($url))) {
  print "YES, they match!\n";
}

function unparse_url($parsed_url) {
  $scheme   = isset($parsed_url['scheme']) ? $parsed_url['scheme'] . '://' : '';
  $host     = isset($parsed_url['host']) ? $parsed_url['host'] : '';
  $port     = isset($parsed_url['port']) ? ':' . $parsed_url['port'] : '';
  $user     = isset($parsed_url['user']) ? $parsed_url['user'] : '';
  $pass     = isset($parsed_url['pass']) ? ':' . $parsed_url['pass']  : '';
  $pass     = ($user || $pass) ? "$pass@" : '';
  $path     = isset($parsed_url['path']) ? $parsed_url['path'] : '';
  $query    = isset($parsed_url['query']) ? '?' . $parsed_url['query'] : '';
  $fragment = isset($parsed_url['fragment']) ? '#' . $parsed_url['fragment'] : '';
  return "$scheme$user$pass$host$port$path$query$fragment";
}

?>
```
  

#

Here is utf-8 compatible parse_url() replacement function based on "laszlo dot janszky at gmail dot com" work. Original incorrectly handled URLs with user:pass. Also made PHP 5.5 compatible (got rid of now deprecated regex /e modifier).<br><br>

```
<?php

    /**
     * UTF-8 aware parse_url() replacement.
     * 
     * @return array
     */
    function mb_parse_url($url)
    {
        $enc_url = preg_replace_callback(
            '%[^:/@?&amp;=#]+%usD',
            function ($matches)
            {
                return urlencode($matches[0]);
            },
            $url
        );
        
        $parts = parse_url($enc_url);
        
        if($parts === false)
        {
            throw new \InvalidArgumentException('Malformed URL: ' . $url);
        }
        
        foreach($parts as $name => $value)
        {
            $parts[$name] = urldecode($value);
        }
        
        return $parts;
    }

?>
```
  

#

It may be worth reminding that the value of the #fragment never gets sent to the server.  Anchors processing is exclusively client-side.  

#

I was writing unit tests and needed to cause this function to kick out an error and return FALSE in order to test a specific execution path. If anyone else needs to force a failure, the following inputs will work:<br><br>

```
<?php
parse_url("http:///example.com");
parse_url("http://:80");
parse_url("http://user@:80");
?>
```
  

#

Here&apos;s a good way to using parse_url () gets the youtube link.<br>This function I used in many works:<br><br>

```
<?php
function youtube($url, $width=560, $height=315, $fullscreen=true)
{
    parse_str( parse_url( $url, PHP_URL_QUERY ), $my_array_of_vars );
    $youtube= '&lt;iframe allowtransparency="true" scrolling="no" width="'.$width.'" height="'.$height.'" src="//www.youtube.com/embed/'.$my_array_of_vars['v'].'" frameborder="0"'.($fullscreen?' allowfullscreen':NULL).'&gt;&lt;/iframe&gt;';
    return $youtube;
}

// show youtube on my page
$url='http://www.youtube.com/watch?v=yvTd6XxgCBE';
 youtube($url, 560, 315, true);
?>
```
<br><br>parse_url () allocates a unique youtube code and  put into iframe link and displayed on your page. The size of the videos choose yourself.<br><br>Enjoy.  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-url.php)

**[To root](/README.md)**