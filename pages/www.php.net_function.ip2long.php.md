# ip2long



A quick method to convert a netmask (ex: 255.255.255.240) to a cidr mask (ex: /28):<br><br>

```
<?php
function mask2cidr($mask){
  $long = ip2long($mask);
  $base = ip2long('255.255.255.255');
  return 32-log(($long ^ $base)+1,2);

  /* xor-ing will give you the inverse mask,
      log base 2 of that +1 will return the number
      of bits that are off in the mask and subtracting
      from 32 gets you the cidr notation */
        
}
?>
```
  

---



```
<?php
/*
     Given an IP address and Subnet Mask, 
     determine the subnet address, broadcast address, and wildcard mask
     by using bitwise operators

     ref:  http://php.net/manual/en/language.operators.bitwise.php
*/

$ip='10.10.10.7';
$mask='255.255.255.0';
$wcmask=long2ip( ~ip2long($mask) );
$subnet=long2ip( ip2long($ip) &amp; ip2long($mask) );
$bcast=long2ip( ip2long($ip) | ip2long($wcmask) );
echo "Given address $ip and mask $mask, \n" . 
"the subnet is $subnet and broadcast is $bcast \n" . 
"with a wildcard mask of $wcmask";

/*
Given address 10.10.10.7 and mask 255.255.255.0, 
the subnet is 10.10.10.0 and broadcast is 10.10.10.255 
with a wildcard mask of 0.0.0.255
*/

?>
```
  

---

To nate, who advises that there is no reason to use an unsigned version of the IP in a MySQL DB:<br><br>I think it would depend on your application, but personally, I find it useful to store IP&apos;s as unsigneds since MySQL has 2 native functions, INET_ATON() and INET_NTOA(), which work the same as ip2long()/long2ip() _except_ that they generate the unsigned counterpart. So if you want, you could do:<br><br>-- IANA Class-B reserved/private<br>SELECT * FROM `servers` <br>WHERE `ip` &gt;= INET_ATON(&apos;192.168.0.0&apos;) <br>AND `ip` &lt;= INET_ATON(&apos;192.168.255.255&apos;);<br><br>In my current application, I find it easier to use the MySQL built-ins than the PHP counter-parts. <br><br>In case you&apos;re curious as to the names ATON and NTOA:<br><br>ATON = address to number aka. ip2long<br>NTOA = number to address aka. long2ip  

---

[Official documentation page](https://www.php.net/manual/en/function.ip2long.php)

**[To root](/README.md)**