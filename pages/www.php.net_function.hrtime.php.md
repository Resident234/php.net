# hrtime



This function is particularly necessary on VMs running on KVM, XEN (openstack, AWS EC2, etc) when timing execution times. <br><br>On these platforms which lack vDSO the common method of using time() or microtime() can dramatically increase CPU/execution time due to the context switching from userland to kernel when running the `gettimeofday()` system call.<br><br>The common pattern is:<br>

```
<?php
$time = -microtime(true);
sleep(5);
$end = sprintf('%f', $time += microtime(true));
?>
```


Substituted as:


```
<?php
$start=hrtime(true); 
sleep(5); 
$end=hrtime(true);
$eta=$end-$start;

echo $eta/1e+6; //nanoseconds to milliseconds
//5000.362419

//OR simply

$eta=-hrtime(true);
sleep(5);
$eta+=hrtime(true);

echo $eta/1e+6; //nanoseconds to milliseconds
//5000.088229
?>
```
<br><br>There is also the new StopWatch class http://php.net/manual/en/class.hrtime-stopwatch.php  

#

[Official documentation page](https://www.php.net/manual/en/function.hrtime.php)

**[To root](/README.md)**