# Basic curl example



IMO this example would have been better if it had done a check for curl_error(), in order to advertize the existence of this function to people learning about cURL who try the example but mysteriously get no response back for whatever reason.  

#

It is important to notice that when using curl to post form data and you use an array for CURLOPT_POSTFIELDS option, the post will be in multipart format<br><br>

```
<?php
$params=['name'=>'John', 'surname'=>'Doe', 'age'=>36)
$defaults = array(
CURLOPT_URL => 'http://myremoteservice/', 
CURLOPT_POST => true,
CURLOPT_POSTFIELDS => $params,
);
$ch = curl_init();
curl_setopt_array($ch, ($options + $defaults));
?>
```
<br>This produce the following post header:<br><br>--------------------------fd1c4191862e3566<br>Content-Disposition: form-data; name="name"<br><br>Jhon<br>--------------------------fd1c4191862e3566<br>Content-Disposition: form-data; name="surnname"<br><br>Doe<br>--------------------------fd1c4191862e3566<br>Content-Disposition: form-data; name="age"<br><br>36<br>--------------------------fd1c4191862e3566--<br><br>Setting CURLOPT_POSTFIELDS as follow produce a standard post header<br><br>CURLOPT_POSTFIELDS =&gt; http_build_query($params),<br><br>Which is:<br>name=John&amp;surname=Doe&amp;age=36<br><br>This caused me 2 days of debug while interacting with a java service which was sensible to this difference, while the equivalent one in php got both format without problem.  

#

[Official documentation page](https://www.php.net/manual/en/curl.examples-basic.php)

**[To root](/README.md)**