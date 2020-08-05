# stream_socket_recvfrom



Note that stream_socket_recvfrom() bypasses stream wrappers including TLS/SSL. While reading from an encrypted stream with fread() will return decrypted data, using stream_socket_recvfrom() will give you the original encrypted bytes.  

---

[Official documentation page](https://www.php.net/manual/en/function.stream-socket-recvfrom.php)

**[To root](/README.md)**