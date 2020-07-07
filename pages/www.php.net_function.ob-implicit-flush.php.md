# ob_implicit_flush





Note that the name ob_implicit_flush is misleading. Despite its name, this function does NOT work with the user output buffer, i.e. the one that the rest of the ob_* functions work with. It will NOT do an automatic ob_flush(). It will do an automatic flush(). Different things.

For example, the following script:



```
<?php
&#xA0; ob_implicit_flush();
&#xA0; for($i = 0; $i &lt; 10; $i++)
&#xA0; {
&#xA0; &#xA0; echo &quot;$i\n&quot;;
&#xA0; &#xA0; sleep(1);
&#xA0; }
php?>
```


will be equivalent to this one:



```
<?php
&#xA0; for ($i = 0; $i &lt; 10; $i++)
&#xA0; {
&#xA0; &#xA0; echo &quot;$i\n&quot;;
&#xA0; &#xA0; flush();
&#xA0; &#xA0; sleep(1);
&#xA0; }
php?>
```


That script will not output anything until the end, if &apos;output_buffering&apos; is set to &apos;on&apos; in php.ini. Unfortunately, there is no way to do an implicit ob_flush() after each output, that I am aware of.

If you want the output to come out as it is generated, one solution is to *also* add ob_end_clean() or ob_end_flush() to the beginning of the script:



```
<?php
&#xA0; ob_end_flush();
&#xA0; ob_implicit_flush();
&#xA0; for ($i = 0; $i &lt; 10; $i++)
&#xA0; {
&#xA0; &#xA0; echo &quot;$i\n&quot;;
&#xA0; &#xA0; sleep(1);
&#xA0; }
php?>
```


This will output as it goes. This is only a problem if you only want one part of the output to come out in real time, and want the rest buffered. In that case, since there&apos;s no function to do an implicit ob_flush() every time, you need to call it explicitly. For example, this works:



```
<?php
&#xA0; ob_start(); // not needed if output_buffering is on in php.ini
&#xA0; ob_implicit_flush(); // implicitly calls flush() after every ob_flush()

&#xA0; echo &quot;This output is buffered.\n&quot;;
&#xA0; echo &quot;As is this.\n&quot;;

&#xA0; for ($i = 0; $i &lt; 10; $i++)
&#xA0; {
&#xA0; &#xA0; echo &quot;$i\n&quot;;
&#xA0; &#xA0; ob_flush();
&#xA0; &#xA0; sleep(1);
&#xA0; }
php?>
```


Note also that some browsers may wait until they have a certain amount of output. See flush [ http://php.net/manual/en/function.flush.php ] for details. It was my case with Firefox (Iceweasel 17.0); unless I output 1024 bytes at the beginning, it does not begin to output.

  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-implicit-flush.php)

**[To root](/README.md)**