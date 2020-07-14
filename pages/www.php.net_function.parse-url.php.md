# parse_url



[If you haven&apos;t yet] been able to find a simple conversion back to string from a parsed url, here&apos;s an example:<br><br>

```
<?php

$url = &apos;http://usr:pss@example.com:81/mypath/myfile.html?a=b&amp;b[]=2&amp;b[]=3#myfragment&apos;;
if ($url === unparse_url(parse_url($url))) {
  print "YES, they match!\n";
}

function unparse_url($parsed_url) {
  $scheme   = isset($parsed_url[&apos;scheme&apos;]) ? $parsed_url[&apos;scheme&apos;] . &apos;://&apos; : &apos;&apos;;
  $host     = isset($parsed_url[&apos;host&apos;]) ? $parsed_url[&apos;host&apos;] : &apos;&apos;;
  $port     = isset($parsed_url[&apos;port&apos;]) ? &apos;:&apos; . $parsed_url[&apos;port&apos;] : &apos;&apos;;
  $user     = isset($parsed_url[&apos;user&apos;]) ? $parsed_url[&apos;user&apos;] : &apos;&apos;;
  $pass     = isset($parsed_url[&apos;pass&apos;]) ? &apos;:&apos; . $parsed_url[&apos;pass&apos;]  : &apos;&apos;;
  $pass     = ($user || $pass) ? "$pass@" : &apos;&apos;;
  $path     = isset($parsed_url[&apos;path&apos;]) ? $parsed_url[&apos;path&apos;] : &apos;&apos;;
  $query    = isset($parsed_url[&apos;query&apos;]) ? &apos;?&apos; . $parsed_url[&apos;query&apos;] : &apos;&apos;;
  $fragment = isset($parsed_url[&apos;fragment&apos;]) ? &apos;#&apos; . $parsed_url[&apos;fragment&apos;] : &apos;&apos;;
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
            &apos;%[^:/@?&amp;=#]+%usD&apos;,
            function ($matches)
            {
                return urlencode($matches[0]);
            },
            $url
        );
        
        $parts = parse_url($enc_url);
        
        if($parts === false)
        {
            throw new \InvalidArgumentException(&apos;Malformed URL: &apos; . $url);
        }
        
        foreach($parts as $name =&gt; $value)
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
    $youtube= &apos;&lt;iframe allowtransparency="true" scrolling="no" width="&apos;.$width.&apos;" height="&apos;.$height.&apos;" src="//www.youtube.com/embed/&apos;.$my_array_of_vars[&apos;v&apos;].&apos;" frameborder="0"&apos;.($fullscreen?&apos; allowfullscreen&apos;:NULL).&apos;&gt;&lt;/iframe&gt;&apos;;
    return $youtube;
}

// show youtube on my page
$url=&apos;http://www.youtube.com/watch?v=yvTd6XxgCBE&apos;;
 youtube($url, 560, 315, true);
?>
```
<br><br>parse_url () allocates a unique youtube code and  put into iframe link and displayed on your page. The size of the videos choose yourself.<br><br>Enjoy.  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-url.php)

**[To root](/README.md)**