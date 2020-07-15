# rawurlencode



You can encode paths using:<br><br>

```
<?php
$encoded = implode("/", array_map("rawurlencode", explode("/", $path)));
?>
```
  

#

I&apos;ve written a simple function to convert an UTF-8 string to URL encoded string. All the given characters are converted!<br><br>The function:<br>

```
<?php
function mb_rawurlencode($url){
$encoded='';
$length=mb_strlen($url);
for($i=0;$i<$length;$i++){
$encoded.='%'.wordwrap(bin2hex(mb_substr($url,$i,1)),2,'%',true);
}
return $encoded;
}
?>
```


Example:


```
<?php
echo 'http://example.com/',
    mb_rawurlencode('&#x4F60;&#x597D;');
?>
```
<br><br>The above example will output:<br>http://example.com/%e4%bd%a0%e5%a5%bd  

#

rawurlencode() MUST not be used on unparsed URLs.<br><br>rawurlencode() should not be used on host and domain name parts (that may include international characters encoded in each domain part with a "q--" prefix followed by a special encoding of the international domain, currently in testbed).<br><br>rawurlencode() may be used on usernames and passwords separately (so that it won&apos;t encode the &apos;:&apos; and &apos;@&apos; separators).<br><br>rawurlencode() must not be used on paths (that may contain &apos;/&apos; separators): the [&apos;path&apos;] element of a parsed URL must first be exploded into individual "directory" names. A directory or filename that contains a space must not be encoded with urlencode() but with this rawurlencode(), so that it will appear as a &apos;%20&apos; hex sequence (not &apos;+&apos;)<br><br>rawurlencode() must not be used to encode the [&apos;query&apos;] element of a parsed URL. Instead you must use the urlencode() function:<br><br>Typical queries often use the &apos;&amp;&apos; separator between each parameter. This &apos;&amp;&apos; separator however is just a convention, used in the www-url-encoded format for HTML forms using the default GET method. However, when references are done in a HTML page to an URL that contains static query parameters, these &apos;&amp;&apos; separators should be encoded in the HTML code as &apos;&amp;amp;&apos; for HTML conformance. This is not part of the URL specification, but of the HTML encapsulation! Some browsers forget this, and send &apos;&amp;amp;&apos; with their HTTP GET query. You may wish to substitute &apos;&amp;amp;&apos; by &apos;&amp;&apos; when parsing and validating URLs. This should be done BEFORE calling urlencode() on query parts.<br><br>The [&apos;fragment&apos;] part of a parsed URL (after the first &apos;#&apos; separator found in any URL) must not be encoded with this rawurlencode() function but instead by urlencode().<br><br>Validating a URL sent in a HTTP request is then more complicated than what you may think. This must be done only on parsed URLs (where the basic elements of an URL have been splitted), and then you must explode the path components, and check the presence of &apos;&amp;amp;&apos; sequences in the query or fragment parts.<br><br>The next thing to do is to check the URL scheme that you want to support (for example, only &apos;http&apos;, &apos;https&apos;, or &apos;ftp&apos;).<br><br>You may wich to check the [&apos;port&apos;] part to see if it&apos;s really a decimal integer between 1 and 65535.<br>You may wish to remove the default port number used by the URL schemes you want to support (for example the port &apos;80&apos; for &apos;http&apos;, the port &apos;21&apos; for &apos;ftp&apos;, the port &apos;443&apos; for &apos;https&apos;), and restrict severely all port numbers below 1024, or some critical ports below 140 (this includes DNS and NetBios ports).<br><br>Then you may wish to control severely the [&apos;host&apos;] part (in fact a full host domain name or an IP address), by forbidding those host names that don&apos;t contain at least one dot, forbidding those that start with a dot, those that contain two consecutive dots, those that start or finish with a &apos;-&apos; dash, those that contain &apos;.-&apos; or &apos;-.&apos; (invalid in all domain names), those that contain two dashes in another position than the second and third character of a domain name part and not folled by at least one other character, forbid top level domain names that have only one non numeric character, or more than 6 characters (".museum" is, for now, the longest acceptable TLD), check that pseudo-TLD names that are pure integers are effectively between 0 and 255, in that case check that this is a valid IPv4 address by comparing it to long2ip(ip2long($host)), ...<br><br>This done, you must use the urlencode() function on all parts up to the exploded path elements, and rawurlencode() on the query and fragment parts, according to the specs, to recreate a complete and validated URL.  

#

[Official documentation page](https://www.php.net/manual/en/function.rawurlencode.php)

**[To root](/README.md)**