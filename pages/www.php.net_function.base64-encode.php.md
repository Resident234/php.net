# base64_encode



For anyone interested in the &apos;base64url&apos; variant encoding, you can use this pair of functions:<br><br>

```
<?php
function base64url_encode($data) {
  return rtrim(strtr(base64_encode($data), '+/', '-_'), '=');
}

function base64url_decode($data) {
  return base64_decode(str_pad(strtr($data, '-_', '+/'), strlen($data) % 4, '=', STR_PAD_RIGHT));
}
?>
```
  

---

In PHP 7, the padding issue with base64_decode() is no more - the following is totally fine:<br><br>function base64_encode_url($string) {<br>    return str_replace([&apos;+&apos;,&apos;/&apos;,&apos;=&apos;], [&apos;-&apos;,&apos;_&apos;,&apos;&apos;], base64_encode($string));<br>}<br><br>function base64_decode_url($string) {<br>    return base64_decode(str_replace([&apos;-&apos;,&apos;_&apos;], [&apos;+&apos;,&apos;/&apos;], $string));<br>}<br><br>Checked here with random_bytes() and random lengths:<br><br>https://3v4l.org/aEs4o  

---

gutzmer at usa dot net&apos;s ( http://php.net/manual/en/function.base64-encode.php#103849 ) base64url_decode() function doesn&apos;t pad longer strings with &apos;=&apos;s. Here is a corrected version: <br><br>

```
<?php
function base64url_encode( $data ){
  return rtrim( strtr( base64_encode( $data ), '+/', '-_'), '=');
}

function base64url_decode( $data ){
  return base64_decode( strtr( $data, '-_', '+/') . str_repeat('=', 3 - ( 3 + strlen( $data )) % 4 ));
}

// proof
for( $i = 0, $s = ''; $i < 24; ++$i, $s .= substr("$i", -1 )){
  $base64_encoded    = base64_encode(    $s );
  $base64url_encoded = base64url_encode( $s );
  $base64url_decoded = base64url_decode( $base64url_encoded );
  $base64_restored   = strtr( $base64url_encoded, '-_', '+/')
                     . str_repeat('=',
                         3 - ( 3 + strlen( $base64url_encoded )) % 4
                       );
  echo "$s<br>$base64url_decoded<br>$base64_encoded<br>$base64_restored<br>$base64url_encoded<br><br>";
}
?>
```
  

---

Base64 encoding of large files.<br><br>Base64 encoding converts triples of eight-bit symbols into quadruples of six-bit symbols. Reading the input file in chunks that are a multiple of three bytes in length results in a chunk that can be encoded independently of the rest of the input file. MIME additionally enforces a line length of 76 characters plus the CRLF. 76 characters is enough for 19 quadruples of six-bit symbols thus representing 19 triples of eight-bit symbols. Reading 57 eight-bit symbols provides exactly enough data for a complete MIME-formatted line. Finally, PHP&apos;s default buffer size is 8192 bytes - enough for 143 MIME lines&apos; worth of input.<br><br>So if you read from the input file in chunks of 8151 (=57*143) bytes you will get (up to) 8151 eight-bit symbols, which encode as exactly 10868 six-bit symbols, which then wrap to exactly 143 MIME-formatted lines. There is no need to retain left-over symbols (either six- or eight-bit) from one chunk to the next. Just read a chunk, encode it, write it out, and go on to the next chunk. Obviously the last chunk will probably be shorter, but encoding it is still independent of the rest.<br><br>

```
<?php

while(!feof($input_file))
{
    $plain = fread($input_file, 57 * 143);
    $encoded = base64_encode($plain);
    $encoded = chunk_split($encoded, 76, "\r\n");
    fwrite($output_file, $encoded);
}

?>
```
<br><br>Conversely, each 76-character MIME-formatted line (not counting the trailing CRLF) contains exactly enough data for 57 bytes of output without needing to retain leftover bits that need prepending to the next line. What that means is that each line can be decoded independently of the others, and the decoded chunks can then be concatenated together or written out sequentially. However, this does make the assumption that the encoded data really is MIME-formatted; without that assurance it is necessary to accept that the base64 data won&apos;t be so conveniently arranged.  

---

A function I&apos;m using to return local images as base64 encrypted code, i.e. embedding the image source into the html request.<br><br>This will greatly reduce your page load time as the browser will only need to send one server request for the entire page, rather than multiple requests for the HTML and the images. Requests need to be uploaded and 99% of the world are limited on their upload speed to the server.<br><br>

```
<?php 
function base64_encode_image ($filename=string,$filetype=string) {
    if ($filename) {
        $imgbinary = fread(fopen($filename, "r"), filesize($filename));
        return 'data:image/' . $filetype . ';base64,' . base64_encode($imgbinary);
    }
}
?>
```


used as so

<style type="text/css">
.logo {
    background: url("

```
<?php echo base64_encode_image ('img/logo.png','png'); ?>
```
") no-repeat right 5px;
}
</style>

or

<img src="

```
<?php echo base64_encode_image ('img/logo.png','png'); ?>
```
"/&gt;  

---

Unfortunately my "function" for encoding base64 on-the-fly from 2007 [which has been removed from the manual in favor of this post] had 2 errors!<br>The first led to an endless loop because of a missing "$feof"-check, the second caused the rare mentioned errors when encoding failed for some reason in larger files, especially when<br>setting fgets($fh, 2) for example. But lower values then 1024 are bad overall because they slow down the whole process, so 4096 will be fine for all purposes, I guess.<br>The error was caused by the use of "empty()".<br><br>Here comes the corrected version which I have tested for all kind of files and length (up to 4,5 Gb!) without any error:<br><br>

```
<?php
$fh = fopen('Input-File', 'rb');
//$fh2 = fopen('Output-File', 'wb');

$cache = '';
$eof = false;

while (1) {

    if (!$eof) {
        if (!feof($fh)) {
            $row = fgets($fh, 4096);
        } else {
            $row = '';
            $eof = true;
        }
    }

    if ($cache !== '')
        $row = $cache.$row;
    elseif ($eof)
        break;

    $b64 = base64_encode($row);
    $put = '';

    if (strlen($b64) < 76) {
        if ($eof) {
            $put = $b64."\n";
            $cache = '';
        } else {
            $cache = $row;
        }

    } elseif (strlen($b64) > 76) {
        do {
            $put .= substr($b64, 0, 76)."\n";
            $b64 = substr($b64, 76);
        } while (strlen($b64) > 76);

        $cache = base64_decode($b64);

    } else {
        if (!$eof &amp;&amp; $b64{75} == '=') {
            $cache = $row;
        } else {
            $put = $b64."\n";
            $cache = '';
        }
    }

    if ($put !== '') {
        echo $put;
        //fputs($fh2, $put);
        //fputs($fh2, base64_decode($put));        // for comparing
    }
}

//fclose($fh2);
fclose($fh);
?>
```
  

---

function urlsafe_b64encode($string) {<br>    $data = base64_encode($string);<br>    $data = str_replace(array(&apos;+&apos;,&apos;/&apos;,&apos;=&apos;),array(&apos;-&apos;,&apos;_&apos;,&apos;&apos;),$data);<br>    return $data;<br>}<br><br>function urlsafe_b64decode($string) {<br>    $data = str_replace(array(&apos;-&apos;,&apos;_&apos;),array(&apos;+&apos;,&apos;/&apos;),$string);<br>    $mod4 = strlen($data) % 4;<br>    if ($mod4) {<br>        $data .= substr(&apos;====&apos;, $mod4);<br>    }<br>    return base64_decode($data);<br>}<br><br>Php version of perl&apos;s MIME::Base64::URLSafe, that provides an url-safe base64 string encoding/decoding (compatible with python base64&apos;s urlsafe methods)  

---

[Official documentation page](https://www.php.net/manual/en/function.base64-encode.php)

**[To root](/README.md)**