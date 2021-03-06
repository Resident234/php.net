# file_exists



Note: The results of this function are cached. See clearstatcache() for more details.<br><br>That&apos;s a pretty big note. Don&apos;t forget this one, since it can make your file_exists() behave unexpectedly - probably at production time ;)  

---

I needed to measure performance for a project, so I did a simple test with one million file_exists() and is_file() checks. In one scenario, only seven of the files existed. In the second, all files existed. is_file() needed 3.0 for scenario one and 3.3 seconds for scenario two.  file_exists() needed 2.8 and 2.9 seconds, respectively. The absolute numbers are off course system-dependant, but it clearly indicates that file_exists() is faster.  

---

In response to seejohnrun&apos;s version to check if a URL exists. Even if the file doesn&apos;t exist you&apos;re still going to get 404 headers.  You can still use get_headers if you don&apos;t have the option of using CURL..<br><br>$file = &apos;http://www.domain.com/somefile.jpg&apos;;<br>$file_headers = @get_headers($file);<br>if($file_headers[0] == &apos;HTTP/1.1 404 Not Found&apos;) {<br>    $exists = false;<br>}<br>else {<br>    $exists = true;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.file-exists.php)

**[To root](/README.md)**