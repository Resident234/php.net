# header



I strongly recommend, that you use<br><br>header($_SERVER["SERVER_PROTOCOL"]." 404 Not Found");<br><br>instead of <br><br>header("HTTP/1.1 404 Not Found");<br><br>I had big troubles with an Apache/2.0.59 (Unix) answering in HTTP/1.0 while I (accidentially) added a "HTTP/1.1 200 Ok" - Header.<br><br>Most of the pages were displayed correct, but on some of them apache added weird content to it:<br><br>A 4-digits HexCode on top of the page (before any output of my php script), seems to be some kind of checksum, because it changes from page to page and browser to browser. (same code for same page and browser)<br><br>"0" at the bottom of the page (after the complete output of my php script) <br><br>It took me quite a while to find out about the wrong protocol in the HTTP-header.  

---

Several times this one is asked on the net but an answer could not be found in the docs on php.net ...<br><br>If you want to redirect an user and tell him he will be redirected, e. g. "You will be redirected in about 5 secs. If not, click here." you cannot use header( &apos;Location: ...&apos; ) as you can&apos;t sent any output before the headers are sent.<br><br>So, either you have to use the HTML meta refresh thingy or you use the following:<br><br>

```
<?php
  header( "refresh:5;url=wherever.php" );
  echo 'You\'ll be redirected in about 5 secs. If not, click <a href="wherever.php">here</a>.';
?>
```
<br><br>Hth someone  

---

A quick way to make redirects permanent or temporary is to make use of the $http_response_code parameter in header().<br><br>

```
<?php
// 301 Moved Permanently
header("Location: /foo.php",TRUE,301);

// 302 Found
header("Location: /foo.php",TRUE,302);
header("Location: /foo.php");

// 303 See Other
header("Location: /foo.php",TRUE,303);

// 307 Temporary Redirect
header("Location: /foo.php",TRUE,307);
?>
```
<br><br>The HTTP status code changes the way browsers and robots handle redirects, so if you are using header(Location:) it&apos;s a good idea to set the status code at the same time.  Browsers typically re-request a 307 page every time, cache a 302 page for the session, and cache a 301 page for longer, or even indefinitely.  Search engines typically transfer "page rank" to the new location for 301 redirects, but not for 302, 303 or 307. If the status code is not specified, header(&apos;Location:&apos;) defaults to 302.  

---

When using PHP to output an image, it won&apos;t be cached by the client so if you don&apos;t want them to download the image each time they reload the page, you will need to emulate part of the HTTP protocol.<br><br>Here&apos;s how:<br><br>

```
<?php

    // Test image.
    $fn = '/test/foo.png';

    // Getting headers sent by the client.
    $headers = apache_request_headers(); 

    // Checking if the client is validating his cache and if it is current.
    if (isset($headers['If-Modified-Since']) &amp;&amp; (strtotime($headers['If-Modified-Since']) == filemtime($fn))) {
        // Client's cache IS current, so we just respond '304 Not Modified'.
        header('Last-Modified: '.gmdate('D, d M Y H:i:s', filemtime($fn)).' GMT', true, 304);
    } else {
        // Image not cached or cache outdated, we respond '200 OK' and output the image.
        header('Last-Modified: '.gmdate('D, d M Y H:i:s', filemtime($fn)).' GMT', true, 200);
        header('Content-Length: '.filesize($fn));
        header('Content-Type: image/png');
        print file_get_contents($fn);
    }

?>
```
<br><br>That way foo.png will be properly cached by the client and you&apos;ll save bandwith. :)  

---

If using the &apos;header&apos; function for the downloading of files, especially if you&apos;re passing the filename as a variable, remember to surround the filename with double quotes, otherwise you&apos;ll have problems in Firefox as soon as there&apos;s a space in the filename.<br><br>So instead of typing:<br><br>

```
<?php
  header("Content-Disposition: attachment; filename=" . basename($filename));
?>
```


you should type:



```
<?php
  header("Content-Disposition: attachment; filename=\"" . basename($filename) . "\"");
?>
```
<br><br>If you don&apos;t do this then when the user clicks on the link for a file named "Example file with spaces.txt", then Firefox&apos;s Save As dialog box will give it the name "Example", and it will have no extension.<br><br>See the page called "Filenames_with_spaces_are_truncated_upon_download" at <br>http://kb.mozillazine.org/ for more information. (Sorry, the site won&apos;t let me post such a long link...)  

---

Be aware that sending binary files to the user-agent (browser) over an encrypted connection (SSL/TLS) will fail in IE (Internet Explorer) versions 5, 6, 7, and 8 if any of the following headers is included:<br><br>Cache-control:no-store<br>Cache-control:no-cache<br><br>See: http://support.microsoft.com/kb/323308<br><br>Workaround: do not send those headers.<br><br>Also, be aware that IE versions 5, 6, 7, and 8 double-compress already-compressed files and do not reverse the process correctly, so ZIP files and similar are corrupted on download.<br><br>Workaround: disable compression (beyond text/html) for these particular versions of IE, e.g., using Apache&apos;s "BrowserMatch" directive. The following example disables compression in all versions of IE:<br><br>BrowserMatch ".*MSIE.*" gzip-only-text/html  

---

[Official documentation page](https://www.php.net/manual/en/function.header.php)

**[To root](/README.md)**