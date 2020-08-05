# Built-in web server



In order to set project specific configuration options, simply add a php.ini file to your project, and then run the built-in server with this flag:<br><br>php -S localhost:8000 -c php.ini<br><br>This is especially helpful for settings that cannot be set at runtime (ini_set()).  

---

It&#x2019;s not mentioned directly, and may not be obvious, but you can also use this to create a virtual host. This, of course, requires the help of your hosts file.<br><br>Here are the steps:<br><br>1    /etc/hosts<br>    127.0.0.1    www.example.com<br><br>2    cd [root folder]<br>    php -S www.example.com:8000<br><br>3    Browser:<br>    http://www.example.com:8000/index.php<br><br>Combined with a simple SQLite database, you have a very handy testing environment.  

---

I painfully experienced behaviour that I can&apos;t seem to find documented here so I wanted to save everyone from repeating my mistake by giving the following heads up:<br><br>When starting php -S on a mac (in my case macOS Sierra) to host a local server, I had trouble with connecting from legacy Java. <br><br>As it turned out, if you started the php server with <br>"php -S localhost:80" <br>the server will be started with ipv6 support only!<br><br>To access it via ipv4, you need to change the start up command like so:<br> "php -S 127.0.0.1:80"<br>which starts server in ipv4 mode only.  

---

If your URI contains a dot, you&apos;ll lose the $_SERVER[&apos;PATH_INFO&apos;] variable, when using the built-in webserver.<br>I wanted to write an API, and use .json ending in the URI-s, but then the framework&apos;s routing mechanism broke, and it took a lot of time to discover that the reason behind it was its router relying on $_SERVER[&apos;PATH_INFO&apos;].<br><br>References:<br>https://bugs.php.net/bug.php?id=61286  

---

On Windows you may find useful to have a phpserver.bat file in shell:sendto with the folowing:<br>explorer http://localhost:8888<br>rem check if arg is file or dir<br>if exist "%~1\" (<br>  php -S localhost:8888 -t "%~1"<br>) else (<br>  php -S localhost:8888 -t "%~dp1"<br>)<br><br>then for fast web testing you only have to SendTo a file or folder to this bat and it will open your explorer and run the server.  

---

[Official documentation page](https://www.php.net/manual/en/features.commandline.webserver.php)

**[To root](/README.md)**