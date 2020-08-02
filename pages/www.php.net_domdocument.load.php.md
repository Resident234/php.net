# DOMDocument::load



I had a problem with loading documents over HTTP. I would get errors looking like this:<br><br>Warning: DOMDocument::load(http://external/document.xml): failed to open stream: HTTP request failed! HTTP/1.1 500 Internal Server Error<br><br>The document would load fine in browsers and using wget. The problem is that DOMDocument::load() on my systems (both OS X and Linux) didn&apos;t send any User-Agent header which for some weird reason made Microsoft-IIS/6.0 respond with the 500 error.<br><br>The solution is found on http://php.net/manual/en/function.libxml-set-streams-context.php : <br><br>

```
<?php
$opts = array(
    'http' => array(
        'user_agent' => 'PHP libxml agent',
    )
);

$context = stream_context_create($opts);
libxml_set_streams_context($context);

// request a file through HTTP
$doc = DOMDocument::load('http://www.example.com/file.xml');
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/domdocument.load.php)

**[To root](/README.md)**