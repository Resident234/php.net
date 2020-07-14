# parse_ini_file



I use the following syntax to secure my config.ini.php file:<br><br>;

```
<?php
;die(); // For further security
;/*

[category]
name="value"

;*/

;?>
```
<br><br>Works like a charm and is both: A valid PHP File and a valid ini-File ;)  

#

Undocumented feature!<br><br>Using ${...} as a value will look to<br>1) an INI setting, or<br>2) an environment variable<br><br>For example,<br><br>

```
<?php

print_r(parse_ini_string(&apos;
php_ext_dir = ${extension_dir}
operating_system = ${OS}
&apos;));

?>
```
<br><br>Array<br>(<br>    [php_ext_dir] =&gt; ./ext/<br>    [operating_system] =&gt; Windows_NT<br>)<br><br>Present in PHP 5.3.2, likely in 5.x, maybe even earlier too.  

#

If your configuration file holds any sensitive information (such as database login details), remember NOT to place it within your document root folder! A common mistake is to replace config.inc.php files, which are formatted in PHP:<br>

```
<?php
$database[&apos;host&apos;] = &apos;localhost&apos;;
// etc...
?>
```
<br><br>With config.ini files which are written in plain text:<br>[database]<br>host = localhost<br><br>The file config.ini can be read by anyone who knows where it&apos;s located, if it&apos;s under your document root folder. Remember to place it above!  

#

Here is a quick parse_ini_file wrapper to add extend support to save typing and redundancy.<br>

```
<?php
    /**
     * Parses INI file adding extends functionality via ":base" postfix on namespace.
     *
     * @param string $filename
     * @return array
     */
    function parse_ini_file_extended($filename) {
        $p_ini = parse_ini_file($filename, true);
        $config = array();
        foreach($p_ini as $namespace =&gt; $properties){
            list($name, $extends) = explode(&apos;:&apos;, $namespace);
            $name = trim($name);
            $extends = trim($extends);
            // create namespace if necessary
            if(!isset($config[$name])) $config[$name] = array();
            // inherit base namespace
            if(isset($p_ini[$extends])){
                foreach($p_ini[$extends] as $prop =&gt; $val)
                    $config[$name][$prop] = $val;
            }
            // overwrite / set current namespace values
            foreach($properties as $prop =&gt; $val)
            $config[$name][$prop] = $val;
        }
        return $config;
    }
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

And for the extra-paranoid like myself, add a rule into your httpd.conf file so that *.ini (or *.inc) in my case can&apos;t be sent to a browser:<br><br>&lt;Files *.inc&gt;  <br>    Order deny,allow<br>    Deny from all<br>&lt;/Files&gt;  

#

To those who were like me looking if this could be used to create an array out of commandline output I offer you the function below (I used it to parse mplayer output).<br><br>If you want it behave exactly the same as parse_ini_file you&apos;ll obviously have to add some code to feed the different sections to this one. Hope it&apos;s of help to someone!<br><br>

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
    $returnArray = array();
    
    foreach( $array as $arrayValue )
    {
        $tmpArray = explode( $string, $arrayValue );
        
        if( count( $tmpArray ) == 1 )
        {
            $returnArray[$tmpArray[0]] = &apos;&apos;;
        }
        else if( count( $tmpArray ) == 2 )
        {
            $returnArray[$tmpArray[0]] = $tmpArray[1];
        }
        else if( count( $tmpArray ) &gt; 2 )
        {
            $implodeBack = array();
            $firstLoop      = true;
            foreach( $tmpArray as $tmpValue )
            {
                if( $firstLoop )
                {
                    $firstLoop = false;
                }
                else
                {
                    $implodeBack[] = $tmpValue;
                }
            }
            print_r( $implodeBack );
            $returnArray[$tmpArray[0]] = implode( &apos;=&apos;, $implodeBack );
        }
    }
    
    return $returnArray;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-ini-file.php)

**[To root](/README.md)**