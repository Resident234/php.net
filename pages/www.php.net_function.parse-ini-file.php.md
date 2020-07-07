# parse_ini_file





I use the following syntax to secure my config.ini.php file:

;

```
<?php
;die(); // For further security
;/*

[category]
name=&quot;value&quot;

;*/

;?>
```


Works like a charm and is both: A valid PHP File and a valid ini-File ;)

  

#



Undocumented feature!

Using ${...} as a value will look to
1) an INI setting, or
2) an environment variable

For example,



```
<?php

print_r(parse_ini_string(&apos;
php_ext_dir = ${extension_dir}
operating_system = ${OS}
&apos;));

?>
```


Array
(
&#xA0; &#xA0; <?php_ext_dir] =&gt; ./ext/
&#xA0; &#xA0; [operating_system] =&gt; Windows_NT
)

Present in PHP 5.3.2, likely in 5.x, maybe even earlier too.

  

#



If your configuration file holds any sensitive information (such as database login details), remember NOT to place it within your document root folder! A common mistake is to replace config.inc.php files, which are formatted in PHP:


```
<?php
$database[&apos;host&apos;] = &apos;localhost&apos;;
// etc...
?>
```


With config.ini files which are written in plain text:
[database]
host = localhost

The file config.ini can be read by anyone who knows where it&apos;s located, if it&apos;s under your document root folder. Remember to place it above!

  

#



Here is a quick parse_ini_file wrapper to add extend support to save typing and redundancy.


```
<?php
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Parses INI file adding extends functionality via &quot;:base&quot; postfix on namespace.
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param string $filename
&#xA0; &#xA0;&#xA0; * @return array
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; function parse_ini_file_extended($filename) {
&#xA0; &#xA0; &#xA0; &#xA0; $p_ini = parse_ini_file($filename, true);
&#xA0; &#xA0; &#xA0; &#xA0; $config = array();
&#xA0; &#xA0; &#xA0; &#xA0; foreach($p_ini as $namespace =&gt; $properties){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($name, $extends) = explode(&apos;:&apos;, $namespace);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $name = trim($name);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $extends = trim($extends);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // create namespace if necessary
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!isset($config[$name])) $config[$name] = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // inherit base namespace
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset($p_ini[$extends])){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($p_ini[$extends] as $prop =&gt; $val)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $config[$name][$prop] = $val;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // overwrite / set current namespace values
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($properties as $prop =&gt; $val)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $config[$name][$prop] = $val;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $config;
&#xA0; &#xA0; }
?>
```


Treats this ini:


```
<?php 
/*
[base]
host=localhost
user=testuser
pass=testpass
database=default

[users:base]
database=users

[archive : base]
database=archive
*/
?>
```

As if it were like this:


```
<?php
/*
[base]
host=localhost
user=testuser
pass=testpass
database=default

[users:base]
host=localhost
user=testuser
pass=testpass
database=users

[archive : base]
host=localhost
user=testuser
pass=testpass
database=archive
*/
?>
```



  

#



And for the extra-paranoid like myself, add a rule into your httpd.conf file so that *.ini (or *.inc) in my case can&apos;t be sent to a browser:

&lt;Files *.inc&gt;&#xA0; 
&#xA0; &#xA0; Order deny,allow
&#xA0; &#xA0; Deny from all
&lt;/Files&gt;

  

#



To those who were like me looking if this could be used to create an array out of commandline output I offer you the function below (I used it to parse mplayer output).

If you want it behave exactly the same as parse_ini_file you&apos;ll obviously have to add some code to feed the different sections to this one. Hope it&apos;s of help to someone!



```
<?php
/**
 * The return is very similar to that of parse_ini_file, but this works off files
 * 
 * Below is an example of what it does, where the first
 * value is what you&apos;d normally want to do, and the second and third things that might
 * happen and in case it does it&apos;s good to know what is going on.
 * 
 * $anArray = array( &apos;default=theValue&apos;, &apos;setting=&apos;, &apos;something=value=value&apos; );
 * explodeExplode( &apos;=&apos;, $anArray );
 * 
 * the return will be 
 * array( &apos;default&apos; =&gt; &apos;theValue&apos;, &apos;setting&apos; =&gt; &apos;&apos;, &apos;something&apos; =&gt; &apos;value=value&apos; );
 * 
 * So the oddities here are, text after the second $string occurence dissapearing
 * and empty values resulting in an empty string.
 * 
 * @return $returnArray array array( &apos;setting&apos; =&gt; &apos;value&apos; )
 * @param $string Object
 * @param $array Object
 */
function explodeExplode( $string, $array )
{
&#xA0; &#xA0; $returnArray = array();
&#xA0; &#xA0; 
&#xA0; &#xA0; foreach( $array as $arrayValue )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $tmpArray = explode( $string, $arrayValue );
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if( count( $tmpArray ) == 1 )
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $returnArray[$tmpArray[0]] = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else if( count( $tmpArray ) == 2 )
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $returnArray[$tmpArray[0]] = $tmpArray[1];
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else if( count( $tmpArray ) &gt; 2 )
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $implodeBack = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $firstLoop&#xA0; &#xA0; &#xA0; = true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach( $tmpArray as $tmpValue )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if( $firstLoop )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $firstLoop = false;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $implodeBack[] = $tmpValue;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print_r( $implodeBack );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $returnArray[$tmpArray[0]] = implode( &apos;=&apos;, $implodeBack );
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; return $returnArray;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-ini-file.php)

**[To root](/README.md)**