# data://



When passing plain string without base64 encoding, do not forget to pass the string through URLENCODE(), because PHP automatically urldecodes all entities inside passed string (and therefore all + get lost, all % entities will be converted to the corresponding characters).<br><br>In this case, PHP is strictly compilant with the RFC 2397. Section 3 states that passes data should be either in base64 encoding or urlencoded.<br><br>VALID USAGE:<br>

```
<?php
$fp = fopen(&apos;data:text/plain,&apos;.urlencode($data), &apos;rb&apos;); // urlencoded data
$fp = fopen(&apos;data:text/plain;base64,&apos;.base64_encode($data), &apos;rb&apos;); // base64 encoded data
?>
```


Demonstration of invalid usage:


```
<?php
$data = &apos;G&#xFC;nther says: 1+1 is 2, 10%40 is 20.&apos;;

$fp = fopen(&apos;data:text/plain,&apos;.$data, &apos;rb&apos;); // INVALID, never do this
echo stream_get_contents($fp);
// G&#xFC;nther says: 1 1 is 2, 10@ is 20. // ERROR

$fp = fopen(&apos;data:text/plain,&apos;.urlencode($data), &apos;rb&apos;); // urlencoded data
echo stream_get_contents($fp);
// G&#xFC;nther says: 1+1 is 2, 10%40 is 20. // OK

// Valid option 1: base64 encoded data
$fp = fopen(&apos;data:text/plain;base64,&apos;.base64_encode($data), &apos;rb&apos;); // base64 encoded data
echo stream_get_contents($fp);
// G&#xFC;nther says: 1+1 is 2, 10%40 is 20. // OK
?>
```
  

#

If you want to create a gd-image directly out of a sql-database-field you might want to use:<br><br>

```
<?php
$jpegimage = imagecreatefromjpeg("data://image/jpeg;base64," . base64_encode($sql_result_array[&apos;imagedata&apos;]));
?>
```
<br><br>this goes also for gif, png, etc using the correct "imagecreatefrom$$$"-function and mime-type.  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.data.php)

**[To root](/README.md)**