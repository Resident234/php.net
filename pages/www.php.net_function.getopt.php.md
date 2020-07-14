# getopt



"phpnotes at kipu dot co dot uk" and "tim at digicol dot de" are both wrong or misleading.  Sean was correct.  Quoted space-containing strings on the command line are one argument.  It has to do with how the shell handles the command line, more than PHP.  PHP&apos;s getopt() is modeled on and probably built upon the Unix/POSIX/C library getopt(3) which treats strings as strings, and does not break them apart on white space.<br><br>Here&apos;s proof:<br><br>$ cat opt.php<br>#! /usr/local/bin/php<br>

```
<?php
$options = getopt("f:");
print_r($options);
?>
```
<br>$ opt.php -f a b c<br>Array<br>(<br>    [f] =&gt; a<br>)<br>$ opt.php -f &apos;a b c&apos;<br>Array<br>(<br>    [f] =&gt; a b c<br>)<br>$ opt.php -f "a b c"<br>Array<br>(<br>    [f] =&gt; a b c<br>)<br>$ opt.php -f a\ b\ c<br>Array<br>(<br>    [f] =&gt; a b c<br>)<br>$  

#

Sometimes you will want to run a script from both the command line and as a web page, for example to debug with better output or a command line version that writes an image to the system but a web version that prints the image in the browser. You can use this function to get the same options whether passed as command line arguments or as $_REQUEST values.<br><br>

```
<?php
/**
 * Get options from the command line or web request
 * 
 * @param string $options
 * @param array $longopts
 * @return array
 */
function getoptreq ($options, $longopts)
{
   if (PHP_SAPI === &apos;cli&apos; || empty($_SERVER[&apos;REMOTE_ADDR&apos;]))  // command line
   {
      return getopt($options, $longopts);
   }
   else if (isset($_REQUEST))  // web script
   {
      $found = array();

      $shortopts = preg_split(&apos;@([a-z0-9][:]{0,2})@i&apos;, $options, 0, PREG_SPLIT_DELIM_CAPTURE | PREG_SPLIT_NO_EMPTY);
      $opts = array_merge($shortopts, $longopts);

      foreach ($opts as $opt)
      {
         if (substr($opt, -2) === &apos;::&apos;)  // optional
         {
            $key = substr($opt, 0, -2);

            if (isset($_REQUEST[$key]) &amp;&amp; !empty($_REQUEST[$key]))
               $found[$key] = $_REQUEST[$key];
            else if (isset($_REQUEST[$key]))
               $found[$key] = false;
         }
         else if (substr($opt, -1) === &apos;:&apos;)  // required value
         {
            $key = substr($opt, 0, -1);

            if (isset($_REQUEST[$key]) &amp;&amp; !empty($_REQUEST[$key]))
               $found[$key] = $_REQUEST[$key];
         }
         else if (ctype_alnum($opt))  // no value
         {
            if (isset($_REQUEST[$opt]))
               $found[$opt] = false;
         }
      }

      return $found;
   }

   return false;
}
?>
```


Example



```
<?php
// php script.php -a -c=XXX -e=YYY -f --two --four=ZZZ --five=5
// script.php?a&amp;c=XXX&amp;e=YYY&amp;f&amp;two&amp;four=ZZZ&amp;five=5

$opts = getoptreq(&apos;abc:d:e::f::&apos;, array(&apos;one&apos;, &apos;two&apos;, &apos;three:&apos;, &apos;four:&apos;, &apos;five::&apos;));

var_dump($opts);

/**
array(7) {
  &apos;a&apos; =&gt; bool(false)
  &apos;c&apos; =&gt; string(3) "XXX"
  &apos;e&apos; =&gt; string(3) "YYY"
  &apos;f&apos; =&gt; bool(false)
  &apos;two&apos; =&gt; bool(false)
  &apos;four&apos; =&gt; string(3) "ZZZ"
  &apos;five&apos; =&gt; string(1) "5"
}
*/
?>
```
  

#

Be sure to wrap your head around this PHP jewel that took me a while to comprehend:<br><br>The returned array will contain a boolean FALSE for options that HAVE been specified.<br><br>Because why use TRUE to indicate "yes, it&apos;s there" when you can also use FALSE for that purpose, right? This is completely counter-intuitive and is most certainly only ever possible in PHP-land.  

#

getopt() only returns the options specified if they were listed in the options.<br><br>So you cant make a switch() use default: to complain of an unknown option. :(  

#

[Official documentation page](https://www.php.net/manual/en/function.getopt.php)

**[To root](/README.md)**