# FTP



For those who dont want to deal with handling the connection once created, here is a simple class that allows you to call any ftp function as if it were an extended method.  It automatically puts the ftp connection into the first argument slot (as all ftp functions require).<br><br>This code is php 5.3+<br><br>

```
<?php
class ftp{
    public $conn;

    public function __construct($url){
        $this-&gt;conn = ftp_connect($url);
    }
    
    public function __call($func,$a){
        if(strstr($func,&apos;ftp_&apos;) !== false &amp;&amp; function_exists($func)){
            array_unshift($a,$this-&gt;conn);
            return call_user_func_array($func,$a);
        }else{
            // replace with your own error handler.
            die("$func is not a valid FTP function");
        }
    }
}

// Example
$ftp = new ftp(&apos;ftp.example.com&apos;);
$ftp-&gt;ftp_login(&apos;username&apos;,&apos;password&apos;);
var_dump($ftp-&gt;ftp_nlist());
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/book.ftp.php)

**[To root](/README.md)**