# apache_request_headers



I didn&apos;t found a replacement for apache_request_headers() in PHP::Compat (http://pear.php.net/package/PHP_Compat) so I wrote my own:<br><br>

```
<?php
if( !function_exists('apache_request_headers') ) {
///
function apache_request_headers() {
  $arh = array();
  $rx_http = '/\AHTTP_/';
  foreach($_SERVER as $key => $val) {
    if( preg_match($rx_http, $key) ) {
      $arh_key = preg_replace($rx_http, '', $key);
      $rx_matches = array();
      // do some nasty string manipulations to restore the original letter case
      // this should work in most cases
      $rx_matches = explode('_', $arh_key);
      if( count($rx_matches) &gt; 0 and strlen($arh_key) &gt; 2 ) {
        foreach($rx_matches as $ak_key => $ak_val) $rx_matches[$ak_key] = ucfirst($ak_val);
        $arh_key = implode('-', $rx_matches);
      }
      $arh[$arh_key] = $val;
    }
  }
  return( $arh );
}
///
}
///
?>
```
  

#

There is a simple way to get request headers from Apache even on PHP running as a CGI. As far as I know, it&apos;s the only way to get the headers "If-Modified-Since" and "If-None-Match" when apache_request_headers() isn&apos;t available. You need mod_rewrite, which most web hosts seem to have enabled. Put this in an .htacess file in your web root:<br><br>RewriteEngine on<br>RewriteRule .* - [E=HTTP_IF_MODIFIED_SINCE:%{HTTP:If-Modified-Since}]<br>RewriteRule .* - [E=HTTP_IF_NONE_MATCH:%{HTTP:If-None-Match}]<br><br>The headers are then available in PHP as<br>

```
<?php
  $_SERVER['HTTP_IF_MODIFIED_SINCE'];
  $_SERVER['HTTP_IF_NONE_MATCH'];
?>
```
<br><br>I&apos;ve tested this on PHP/5.1.6, on both Apache/2.2.3/Win32 and Apache/2.0.54/Unix, and it works perfectly.<br><br>Note: if you use RewriteRules already for clean URLs, you need to put the above rules AFTER your existing ones.  

#

[Official documentation page](https://www.php.net/manual/en/function.apache-request-headers.php)

**[To root](/README.md)**