# get_headers



Seems like there are some people who are looking for only the 3-digit HTTP response code  - here is a quick and nasty solution:<br><br>

```
<?php
function get_http_response_code($theURL) {
    $headers = get_headers($theURL);
    return substr($headers[0], 9, 3);
}
?>
```
<br><br>How easy is that? Echo the function containing the URL you want to check the response code for, and voil&#xE0;. Custom redirects, alternative for blocked is_file() or flie_exists() functions (like I seem to have on my servers) hence the cheap workaround. But hey - it works!<br><br>Pudding  

---

[Official documentation page](https://www.php.net/manual/en/function.get-headers.php)

**[To root](/README.md)**