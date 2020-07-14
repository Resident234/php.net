# phpinfo



After reading and trying various functions, I couldn&apos;t find one that correctly parses all the configurations, strips any left-over html tag and converts special characters into UTF8 (e.g. &amp;#039; into &apos;), so I created my own by improving on the existing ones:<br><br>function phpinfo2array() {<br>    $entitiesToUtf8 = function($input) {<br>        // http://php.net/manual/en/function.html-entity-decode.php#104617<br>        return preg_replace_callback("/(&amp;#[0-9]+;)/", function($m) { return mb_convert_encoding($m[1], "UTF-8", "HTML-ENTITIES"); }, $input);<br>    };<br>    $plainText = function($input) use ($entitiesToUtf8) {<br>        return trim(html_entity_decode($entitiesToUtf8(strip_tags($input))));<br>    };<br>    $titlePlainText = function($input) use ($plainText) {<br>        return &apos;# &apos;.$plainText($input);<br>    };<br>    <br>    ob_start();<br>    phpinfo(-1);<br>    <br>    $phpinfo = array(&apos;phpinfo&apos; =&gt; array());<br><br>    // Strip everything after the &lt;h1&gt;Configuration&lt;/h1&gt; tag (other h1&apos;s)<br>    if (!preg_match(&apos;#(.*&lt;h1[^&gt;]*&gt;\s*Configuration.*)&lt;h1#s&apos;, ob_get_clean(), $matches)) {<br>        return array();<br>    }<br>    <br>    $input = $matches[1];<br>    $matches = array();<br><br>    if(preg_match_all(<br>        &apos;#(?:&lt;h2.*?>
```
(?:&lt;a.*?>
```
)?(.*?)(?:&lt;\/a&gt;)?&lt;\/h2&gt;)|&apos;.<br>        &apos;(?:&lt;tr.*?>
```
&lt;t[hd].*?>
```
(.*?)\s*&lt;/t[hd]&gt;(?:&lt;t[hd].*?>
```
(.*?)\s*&lt;/t[hd]&gt;(?:&lt;t[hd].*?>
```
(.*?)\s*&lt;/t[hd]&gt;)?)?&lt;/tr&gt;)#s&apos;,<br>        $input, <br>        $matches, <br>        PREG_SET_ORDER<br>    )) {<br>        foreach ($matches as $match) {<br>            $fn = strpos($match[0], &apos;&lt;th&apos;) === false ? $plainText : $titlePlainText;<br>            if (strlen($match[1])) {<br>                $phpinfo[$match[1]] = array();<br>            } elseif (isset($match[3])) {<br>                $keys1 = array_keys($phpinfo);<br>                $phpinfo[end($keys1)][$fn($match[2])] = isset($match[4]) ? array($fn($match[3]), $fn($match[4])) : $fn($match[3]);<br>            } else {<br>                $keys1 = array_keys($phpinfo);<br>                $phpinfo[end($keys1)][] = $fn($match[2]);<br>            }<br><br>        }<br>    }<br>    <br>    return $phpinfo;<br>}<br><br>The output looks something like this (note the headers are also included but are prefixed with &apos;# &apos;, e.g. &apos;# Directive&apos;):<br><br>Array<br>(<br>    [phpinfo] =&gt; Array<br>        (<br>            [0] =&gt; PHP Version 5.6.5<br>            [System] =&gt; Darwin Calins-MBP 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 19:41:34 PDT 2015; root:xnu-3247.1.106~5/RELEASE_X86_64 x86_64<br>            [Build Date] =&gt; Feb 19 2015 18:34:18<br>            [Registered Stream Socket Transports] =&gt; tcp, udp, unix, udg, ssl, sslv3, sslv2, tls, tlsv1.0<br>            [Registered Stream Filters] =&gt; zlib.*, bzip2.*, convert.iconv.*, string.rot13, string.toupper, string.tolower, string.strip_tags, convert.*, consumed, dechunk<br>            [1] =&gt; This program makes use of the Zend Scripting Language Engine:Zend Engine...<br>        )<br><br>    [apache2handler] =&gt; Array<br>        (<br>            [Apache Version] =&gt; Apache/2.4.16 (Unix) PHP/5.6.5 OpenSSL/0.9.8zg<br>            [Apache API Version] =&gt; 20120211<br>            [Server Administrator] =&gt; webmaster@dummy-host2.example.com<br>            [Hostname:Port] =&gt; sitestacker.local:0<br>            [# Directive] =&gt; Array<br>                (<br>                    [0] =&gt; # Local Value<br>                    [1] =&gt; # Master Value<br>                )<br><br>            [engine] =&gt; Array<br>                (<br>                    [0] =&gt; 1<br>                    [1] =&gt; 1<br>                )<br><br>            [last_modified] =&gt; Array<br>                (<br>                    [0] =&gt; 0<br>                    [1] =&gt; 0<br>                )  

#

[Official documentation page](https://www.php.net/manual/en/function.phpinfo.php)

**[To root](/README.md)**