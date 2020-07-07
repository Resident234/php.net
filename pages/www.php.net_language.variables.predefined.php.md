# Predefined Variables





I haven&apos;t found it anywhere else in the manual, so I&apos;ll make a note of it here - PHP will automatically replace any dots (&apos;.&apos;) in an incoming variable name with underscores (&apos;_&apos;). So if you have dots in your incoming variables, e.g.:

example.com/page.php?chuck.norris=nevercries

you can not reference them by the name used in the URI:
//INCORRECT
echo $_GET[&apos;chuck.norris&apos;];

instead you must use:
//CORRECT
echo $_GET[&apos;chuck_norris&apos;];

  

#



I have this function in my main files, it allows for easier SEO for some pages without having to rely on .htaccess and mod_rewrite for some things.


```
<?php
&#xA0; &#xA0; function long_to_GET(){
&#xA0; &#xA0; &#xA0; &#xA0; /**
&#xA0; &#xA0; &#xA0; &#xA0; * This function converts info.php/a/1/b/2/c?d=4 TO
&#xA0; &#xA0; &#xA0; &#xA0; * Array ( [d] =&gt; 4 [a] =&gt; 1 [b] =&gt; 2 [c] =&gt; ) 
&#xA0; &#xA0; &#xA0; &#xA0; **/
&#xA0; &#xA0; &#xA0; &#xA0; if(isset($_SERVER[&apos;PATH_INFO&apos;]) &amp;&amp; $_SERVER[&apos;PATH_INFO&apos;] != &apos;&apos;){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //Split it out.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $tmp = explode(&apos;/&apos;,$_SERVER[&apos;PATH_INFO&apos;]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //Remove first empty item
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($tmp[0]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //Loop through and apend it into the $_GET superglobal.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for($i=1;$i&lt;=count($tmp);$i+=2){ $_GET[$tmp[$i]] = $tmp[$i+1];}
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
?>
```


Its probably not the most efficient, but it does the job rather nicely.

DD32

  

#



If you&apos;re running PHP as a shell script, and you want to use the argv and argc arrays to get command-line arguments, make sure you have register_argc_argv&#xA0; =&#xA0; on.&#xA0; If you&apos;re using the &apos;optimized&apos; php.ini, this defaults to off.

  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.predefined.php)

**[To root](/README.md)**