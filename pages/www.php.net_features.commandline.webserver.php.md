# Built-in web server





In order to set project specific configuration options, simply add a php.ini file to your project, and then run the built-in server with this flag:

php -S localhost:8000 -c php.ini

This is especially helpful for settings that cannot be set at runtime (ini_set()).

  

#



I painfully experienced behaviour that I can&apos;t seem to find documented here so I wanted to save everyone from repeating my mistake by giving the following heads up:

When starting php -S on a mac (in my case macOS Sierra) to host a local server, I had trouble with connecting from legacy Java. 

As it turned out, if you started the php server with 
&quot;php -S localhost:80&quot; 
the server will be started with ipv6 support only!

To access it via ipv4, you need to change the start up command like so:
 &quot;php -S 127.0.0.1:80&quot;
which starts server in ipv4 mode only.

  

#



It&#x2019;s not mentioned directly, and may not be obvious, but you can also use this to create a virtual host. This, of course, requires the help of your hosts file.

Here are the steps:

1&#xA0; &#xA0; /etc/hosts
&#xA0; &#xA0; 127.0.0.1&#xA0; &#xA0; www.example.com

2&#xA0; &#xA0; cd [root folder]
&#xA0; &#xA0; php -S www.example.com:8000

3&#xA0; &#xA0; Browser:
&#xA0; &#xA0; http://www.example.com:8000/index.php

Combined with a simple SQLite database, you have a very handy testing environment.

  

#



If your URI contains a dot, you&apos;ll lose the $_SERVER[&apos;PATH_INFO&apos;] variable, when using the built-in webserver.
I wanted to write an API, and use .json ending in the URI-s, but then the framework&apos;s routing mechanism broke, and it took a lot of time to discover that the reason behind it was its router relying on $_SERVER[&apos;PATH_INFO&apos;].

References:
https://bugs.php.net/bug.php?id=61286

  

#



On Windows you may find useful to have a phpserver.bat file in shell:sendto with the folowing:
explorer http://localhost:8888
rem check if arg is file or dir
if exist &quot;%~1\&quot; (
&#xA0; php -S localhost:8888 -t &quot;%~1&quot;
) else (
&#xA0; php -S localhost:8888 -t &quot;%~dp1&quot;
)

then for fast web testing you only have to SendTo a file or folder to this bat and it will open your explorer and run the server.

  

#



To output debugging information on the command line you can write output to php://stdout:



```
<?php
$path = $_SERVER[&quot;SCRIPT_FILENAME&quot;];

file_put_contents(&quot;php://stdout&quot;, &quot;\nRequested: $path&quot;);
echo &quot;&lt;p&gt;Hello World&lt;/p&gt;&quot;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.webserver.php)

**[To root](/README.md)**