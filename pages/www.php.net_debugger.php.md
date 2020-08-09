# Debugging in PHP



I find it very useful to print out to the browsers console instead of just var_dumping:<br><br>function console_log( $data ){<br>  echo &apos;&lt;script&gt;&apos;;<br>  echo &apos;console.log(&apos;. json_encode( $data ) .&apos;)&apos;;<br>  echo &apos;&lt;/script&gt;&apos;;<br>}<br><br>Usage:<br>$myvar = array(1,2,3);<br>console_log( $myvar ); // [1,2,3]  

---

A good example of data output to the console via &lt;script&gt; tags, I myself used this first, but he broke the captcha job, because &lt;script&gt; tags were inserted into the base64 code of the captcha picture. Then I began to display logs in the headers with such a function (it may help someone else, in a similar situation):<br><br>

```
<?php

function header_log($data){
  $bt = debug_backtrace();
  $caller = array_shift($bt);
  $line = $caller['line'];
  $file = array_pop(explode('/', $caller['file']));
  header('log_'.$file.'_'.$caller['line'].': '.json_encode($data));
}

?>
```
<br><br>Usage:<br>$myvar = array(1,2,3);<br>header_log( $myvar ); // in headers we see: log_filename_rownumber: [1,2,3]  

---

I would like to add http://phpdebugbar.com/ which is also a handy tool for debugging/profiling data. and you could use it whenever framework you are using. :-)  

---

[Official documentation page](https://www.php.net/manual/en/debugger.php)

**[To root](/README.md)**