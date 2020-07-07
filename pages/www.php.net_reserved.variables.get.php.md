# $_GET





Just a note, because I didn&apos;t know for sure until I tested it.

If you have a query string that contains a parameter but no value (not even an equals sign), like so:
http://path/to/script.php?a

The following script is a good test to determine how a is valued:
&lt;pre&gt;


```
<?php
 print_r($_GET);
 if($_GET[&quot;a&quot;] === &quot;&quot;) echo &quot;a is an empty string\n&quot;;
 if($_GET[&quot;a&quot;] === false) echo &quot;a is false\n&quot;;
 if($_GET[&quot;a&quot;] === null) echo &quot;a is null\n&quot;;
 if(isset($_GET[&quot;a&quot;])) echo &quot;a is set\n&quot;;
 if(!empty($_GET[&quot;a&quot;])) echo &quot;a is not empty&quot;;
?>
```

&lt;/pre&gt;

I tested this with script.php?a, and it returned:

a is an empty string
a is set

So note that a parameter with no value associated with, even without an equals sign, is considered to be an empty string (&quot;&quot;), isset() returns true for it, and it is considered empty, but not false or null. Seems obvious after the first test, but I just had to make sure.

Of course, if I do not include it in my browser query, the script returns
Array
(
)
a is null

  

#



Note that named anchors are not part of the query string and are never submitted by the browser to the server.

Eg.
http://www.xyz-abc.kz/index.php?title=apocalypse.php#doom

echo $_GET[&apos;title&apos;]; 

// returns &quot;apocalypse.php&quot; and NOT &quot;apocalypse.php#doom&quot;

you would be better off treating the named anchor as another query string variable like so:
http://www.xyz-abc.kz/index.php?title=apocalypse.php&amp;na=doom

...and then retrieve it using something like this:
$url = $_GET[&apos;title&apos;].&quot;#&quot;.$_GET[&apos;na&apos;];

Hope this helps someone...

  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.get.php)

**[To root](/README.md)**