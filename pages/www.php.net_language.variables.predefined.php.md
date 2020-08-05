# Predefined Variables



I haven&apos;t found it anywhere else in the manual, so I&apos;ll make a note of it here - PHP will automatically replace any dots (&apos;.&apos;) in an incoming variable name with underscores (&apos;_&apos;). So if you have dots in your incoming variables, e.g.:<br><br>example.com/page.php?chuck.norris=nevercries<br><br>you can not reference them by the name used in the URI:<br>//INCORRECT<br>echo $_GET[&apos;chuck.norris&apos;];<br><br>instead you must use:<br>//CORRECT<br>echo $_GET[&apos;chuck_norris&apos;];  

---

I have this function in my main files, it allows for easier SEO for some pages without having to rely on .htaccess and mod_rewrite for some things.<br>

```
<?php
    function long_to_GET(){
        /**
        * This function converts info.php/a/1/b/2/c?d=4 TO
        * Array ( [d] => 4 [a] => 1 [b] => 2 [c] => ) 
        **/
        if(isset($_SERVER['PATH_INFO']) &amp;&amp; $_SERVER['PATH_INFO'] != ''){
            //Split it out.
            $tmp = explode('/',$_SERVER['PATH_INFO']);
            //Remove first empty item
            unset($tmp[0]);
            //Loop through and apend it into the $_GET superglobal.
            for($i=1;$i<=count($tmp);$i+=2){ $_GET[$tmp[$i]] = $tmp[$i+1];}
        }
    }
?>
```
<br><br>Its probably not the most efficient, but it does the job rather nicely.<br><br>DD32  

---

If you&apos;re running PHP as a shell script, and you want to use the argv and argc arrays to get command-line arguments, make sure you have register_argc_argv  =  on.  If you&apos;re using the &apos;optimized&apos; php.ini, this defaults to off.  

---

[Official documentation page](https://www.php.net/manual/en/language.variables.predefined.php)

**[To root](/README.md)**