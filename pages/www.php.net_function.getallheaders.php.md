# getallheaders



it could be useful if you using nginx instead of apache<br><br>

```
<?php
if (!function_exists('getallheaders')) 
{
    function getallheaders() 
    {
           $headers = [];
       foreach ($_SERVER as $name => $value) 
       {
           if (substr($name, 0, 5) == 'HTTP_') 
           {
               $headers[str_replace(' ', '-', ucwords(strtolower(str_replace('_', ' ', substr($name, 5)))))] = $value;
           }
       }
       return $headers;
    }
}
?>
```
  

#

There&apos;s a polyfill for this that can be downloaded or installed via composer:<br><br>https://github.com/ralouphie/getallheaders  

#

Beware that RFC2616 (HTTP/1.1) defines header fields as case-insensitive entities. Therefore, array keys of getallheaders() should be converted first to lower- or uppercase and processed such.  

#

[Official documentation page](https://www.php.net/manual/en/function.getallheaders.php)

**[To root](/README.md)**