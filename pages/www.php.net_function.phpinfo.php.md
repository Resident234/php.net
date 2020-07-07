# phpinfo





After reading and trying various functions, I couldn&apos;t find one that correctly parses all the configurations, strips any left-over html tag and converts special characters into UTF8 (e.g. &amp;#039; into &apos;), so I created my own by improving on the existing ones:

function phpinfo2array() {
&#xA0; &#xA0; $entitiesToUtf8 = function($input) {
&#xA0; &#xA0; &#xA0; &#xA0; // http://php.net/manual/en/function.html-entity-decode.php#104617
&#xA0; &#xA0; &#xA0; &#xA0; return preg_replace_callback(&quot;/(&amp;#[0-9]+;)/&quot;, function($m) { return mb_convert_encoding($m[1], &quot;UTF-8&quot;, &quot;HTML-ENTITIES&quot;); }, $input);
&#xA0; &#xA0; };
&#xA0; &#xA0; $plainText = function($input) use ($entitiesToUtf8) {
&#xA0; &#xA0; &#xA0; &#xA0; return trim(html_entity_decode($entitiesToUtf8(strip_tags($input))));
&#xA0; &#xA0; };
&#xA0; &#xA0; $titlePlainText = function($input) use ($plainText) {
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;# &apos;.$plainText($input);
&#xA0; &#xA0; };
&#xA0; &#xA0; 
&#xA0; &#xA0; ob_start();
&#xA0; &#xA0; phpinfo(-1);
&#xA0; &#xA0; 
&#xA0; &#xA0; $phpinfo = array(&apos;phpinfo&apos; =&gt; array());

&#xA0; &#xA0; // Strip everything after the &lt;h1&gt;Configuration&lt;/h1&gt; tag (other h1&apos;s)
&#xA0; &#xA0; if (!preg_match(&apos;#(.*&lt;h1[^&gt;]*&gt;\s*Configuration.*)&lt;h1#s&apos;, ob_get_clean(), $matches)) {
&#xA0; &#xA0; &#xA0; &#xA0; return array();
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; $input = $matches[1];
&#xA0; &#xA0; $matches = array();

&#xA0; &#xA0; if(preg_match_all(
&#xA0; &#xA0; &#xA0; &#xA0; &apos;#(?:&lt;h2.*?>
```
(?:&lt;a.*?>
```
)?(.*?)(?:&lt;\/a&gt;)?&lt;\/h2&gt;)|&apos;.
&#xA0; &#xA0; &#xA0; &#xA0; &apos;(?:&lt;tr.*?>
```
&lt;t[hd].*?>
```
(.*?)\s*&lt;/t[hd]&gt;(?:&lt;t[hd].*?>
```
(.*?)\s*&lt;/t[hd]&gt;(?:&lt;t[hd].*?>
```
(.*?)\s*&lt;/t[hd]&gt;)?)?&lt;/tr&gt;)#s&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; $input, 
&#xA0; &#xA0; &#xA0; &#xA0; $matches, 
&#xA0; &#xA0; &#xA0; &#xA0; PREG_SET_ORDER
&#xA0; &#xA0; )) {
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($matches as $match) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fn = strpos($match[0], &apos;&lt;th&apos;) === false ? $plainText : $titlePlainText;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (strlen($match[1])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $phpinfo[$match[1]] = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } elseif (isset($match[3])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $keys1 = array_keys($phpinfo);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $phpinfo[end($keys1)][$fn($match[2])] = isset($match[4]) ? array($fn($match[3]), $fn($match[4])) : $fn($match[3]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $keys1 = array_keys($phpinfo);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $phpinfo[end($keys1)][] = $fn($match[2]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; return $phpinfo;
}

The output looks something like this (note the headers are also included but are prefixed with &apos;# &apos;, e.g. &apos;# Directive&apos;):

Array
(
&#xA0; &#xA0; <?phpinfo] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; PHP Version 5.6.5
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [System] =&gt; Darwin Calins-MBP 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 19:41:34 PDT 2015; root:xnu-3247.1.106~5/RELEASE_X86_64 x86_64
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Build Date] =&gt; Feb 19 2015 18:34:18
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Registered Stream Socket Transports] =&gt; tcp, udp, unix, udg, ssl, sslv3, sslv2, tls, tlsv1.0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Registered Stream Filters] =&gt; zlib.*, bzip2.*, convert.iconv.*, string.rot13, string.toupper, string.tolower, string.strip_tags, convert.*, consumed, dechunk
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; This program makes use of the Zend Scripting Language Engine:Zend&#xA0;Engine...
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; [apache2handler] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Apache Version] =&gt; Apache/2.4.16 (Unix) PHP/5.6.5 OpenSSL/0.9.8zg
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Apache API Version] =&gt; 20120211
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Server Administrator] =&gt; webmaster@dummy-host2.example.com
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [Hostname:Port] =&gt; sitestacker.local:0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [# Directive] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; # Local Value
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; # Master Value
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [engine] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 1
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 1
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [last_modified] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )

  

#

[Official documentation page](https://www.php.net/manual/en/function.phpinfo.php)

**[To root](/README.md)**