# Internet Domain: TCP, UDP, SSL, and TLS



@pablo dot livardo  :  I think that the problem you found is caused by the difference between the client/server encryption methods used.<br><br>The 465 port is used for SMTPS, and the server starts the encryption immediately it receives your connection. So, your code will work.<br><br>The 587 port is used for Submission (MSA or Mail Submission Agent) which works like the port 25. The server accepts your connection and doesn&apos;t activate the encryption. If you want an encrypted connection on the port 587, you must connect on it without encryption, you must start to dialog with the server (with EHLO) and after that you must ask the server to start the encrypted connection using the STARTTLS command. The server starts the encryption and now you can start as well the encryption on your client. <br><br>So, in few words, you can not use : <br><br>

```
<?php $fp = fsockopen("tls://mail.example.com", 587, $errno, $errstr);  ?>
```
  

but you can use:

 

```
<?php $fp = stream_socket_client("mail.example.com:587", $errno, $errstr); ?>
```
  

and after you send the STARTTLS command, you can enable the crypto:



```
<?php stream_socket_enable_crypto($fp, true, STREAM_CRYPTO_METHOD_SSLv23_CLIENT); ?>
```
<br><br>P.S. My previous note on this page was totally wrong, so I ask the php.net admin to remove it.<br><br>:)  

---

[Official documentation page](https://www.php.net/manual/en/transports.inet.php)

**[To root](/README.md)**