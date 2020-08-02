# ob_implicit_flush



Note that the name ob_implicit_flush is misleading. Despite its name, this function does NOT work with the user output buffer, i.e. the one that the rest of the ob_* functions work with. It will NOT do an automatic ob_flush(). It will do an automatic flush(). Different things.<br><br>For example, the following script:<br><br>

```
<?php
  ob_implicit_flush();
  for($i = 0; $i < 10; $i++)
  {
    echo "$i\n";
    sleep(1);
  }
?>
```


will be equivalent to this one:



```
<?php
  for ($i = 0; $i < 10; $i++)
  {
    echo "$i\n";
    flush();
    sleep(1);
  }
?>
```


That script will not output anything until the end, if 'output_buffering' is set to 'on' in php.ini. Unfortunately, there is no way to do an implicit ob_flush() after each output, that I am aware of.

If you want the output to come out as it is generated, one solution is to *also* add ob_end_clean() or ob_end_flush() to the beginning of the script:



```
<?php
  ob_end_flush();
  ob_implicit_flush();
  for ($i = 0; $i < 10; $i++)
  {
    echo "$i\n";
    sleep(1);
  }
?>
```


This will output as it goes. This is only a problem if you only want one part of the output to come out in real time, and want the rest buffered. In that case, since there's no function to do an implicit ob_flush() every time, you need to call it explicitly. For example, this works:



```
<?php
  ob_start(); // not needed if output_buffering is on in php.ini
  ob_implicit_flush(); // implicitly calls flush() after every ob_flush()

  echo "This output is buffered.\n";
  echo "As is this.\n";

  for ($i = 0; $i < 10; $i++)
  {
    echo "$i\n";
    ob_flush();
    sleep(1);
  }
?>
```
<br><br>Note also that some browsers may wait until they have a certain amount of output. See flush [ http://php.net/manual/en/function.flush.php ] for details. It was my case with Firefox (Iceweasel 17.0); unless I output 1024 bytes at the beginning, it does not begin to output.  

---

[Official documentation page](https://www.php.net/manual/en/function.ob-implicit-flush.php)

**[To root](/README.md)**