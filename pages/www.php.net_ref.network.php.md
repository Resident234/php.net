# Network Functions



A simple and very fast function to check against CIDR.<br><br>Your previous examples are too complicated and involves a lot of functions call.<br><br>Here it is (only with arithmetic operators and call only to ip2long () and split() ):<br>

```
<?php
  function ipCIDRCheck ($IP, $CIDR) {
    list ($net, $mask) = split ("/", $CIDR);
    
    $ip_net = ip2long ($net);
    $ip_mask = ~((1 << (32 - $mask)) - 1);

    $ip_ip = ip2long ($IP);

    $ip_ip_net = $ip_ip &amp; $ip_mask;

    return ($ip_ip_net == $ip_net);
  }
?>
```

call example: 

```
<?php echo ipCheck ("192.168.1.23", "192.168.1.0/24"); ?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/ref.network.php)

**[To root](/README.md)**