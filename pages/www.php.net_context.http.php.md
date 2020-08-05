# HTTP context options



Note that if you set the protocol_version option to 1.1 and the server you are requesting from is configured to use keep-alive connections, the function (fopen, file_get_contents, etc.) will "be slow" and take a long time to complete. This is a feature of the HTTP 1.1 protocol you are unlikely to use with stream contexts in PHP.<br><br>Simply add a "Connection: close" header to the request to eliminate the keep-alive timeout:<br><br>

```
<?php
// php 5.4 : array syntax and header option with array value
$data = file_get_contents('http://www.example.com/', null, stream_context_create([
    'http' => [
        'protocol_version' => 1.1,
        'header'           => [
            'Connection: close',
        ],
    ],
]));
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/context.http.php)

**[To root](/README.md)**