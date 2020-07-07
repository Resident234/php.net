# getopt





&quot;phpnotes at kipu dot co dot uk&quot; and &quot;tim at digicol dot de&quot; are both wrong or misleading.&#xA0; Sean was correct.&#xA0; Quoted space-containing strings on the command line are one argument.&#xA0; It has to do with how the shell handles the command line, more than PHP.&#xA0; PHP&apos;s getopt() is modeled on and probably built upon the Unix/POSIX/C library getopt(3) which treats strings as strings, and does not break them apart on white space.

Here&apos;s proof:

$ cat opt.php
#! /usr/local/bin/php


```
<?php
$options = getopt(&quot;f:&quot;);
print_r($options);
?>
```

$ opt.php -f a b c
Array
(
&#xA0; &#xA0; [f] =&gt; a
)
$ opt.php -f &apos;a b c&apos;
Array
(
&#xA0; &#xA0; [f] =&gt; a b c
)
$ opt.php -f &quot;a b c&quot;
Array
(
&#xA0; &#xA0; [f] =&gt; a b c
)
$ opt.php -f a\ b\ c
Array
(
&#xA0; &#xA0; [f] =&gt; a b c
)
$

  

#



Sometimes you will want to run a script from both the command line and as a web page, for example to debug with better output or a command line version that writes an image to the system but a web version that prints the image in the browser. You can use this function to get the same options whether passed as command line arguments or as $_REQUEST values.



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
&#xA0;&#xA0; if (PHP_SAPI === &apos;cli&apos; || empty($_SERVER[&apos;REMOTE_ADDR&apos;]))&#xA0; // command line
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; return getopt($options, $longopts);
&#xA0;&#xA0; }
&#xA0;&#xA0; else if (isset($_REQUEST))&#xA0; // web script
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; $found = array();

&#xA0; &#xA0; &#xA0; $shortopts = preg_split(&apos;@([a-z0-9][:]{0,2})@i&apos;, $options, 0, PREG_SPLIT_DELIM_CAPTURE | PREG_SPLIT_NO_EMPTY);
&#xA0; &#xA0; &#xA0; $opts = array_merge($shortopts, $longopts);

&#xA0; &#xA0; &#xA0; foreach ($opts as $opt)
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (substr($opt, -2) === &apos;::&apos;)&#xA0; // optional
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = substr($opt, 0, -2);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset($_REQUEST[$key]) &amp;&amp; !empty($_REQUEST[$key]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $found[$key] = $_REQUEST[$key];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else if (isset($_REQUEST[$key]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $found[$key] = false;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; else if (substr($opt, -1) === &apos;:&apos;)&#xA0; // required value
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = substr($opt, 0, -1);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset($_REQUEST[$key]) &amp;&amp; !empty($_REQUEST[$key]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $found[$key] = $_REQUEST[$key];
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; else if (ctype_alnum($opt))&#xA0; // no value
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset($_REQUEST[$opt]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $found[$opt] = false;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; return $found;
&#xA0;&#xA0; }

&#xA0;&#xA0; return false;
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
&#xA0; &apos;a&apos; =&gt; bool(false)
&#xA0; &apos;c&apos; =&gt; string(3) &quot;XXX&quot;
&#xA0; &apos;e&apos; =&gt; string(3) &quot;YYY&quot;
&#xA0; &apos;f&apos; =&gt; bool(false)
&#xA0; &apos;two&apos; =&gt; bool(false)
&#xA0; &apos;four&apos; =&gt; string(3) &quot;ZZZ&quot;
&#xA0; &apos;five&apos; =&gt; string(1) &quot;5&quot;
}
*/
?>
```



  

#



Be sure to wrap your head around this PHP jewel that took me a while to comprehend:

The returned array will contain a boolean FALSE for options that HAVE been specified.

Because why use TRUE to indicate &quot;yes, it&apos;s there&quot; when you can also use FALSE for that purpose, right? This is completely counter-intuitive and is most certainly only ever possible in PHP-land.

  

#



getopt() only returns the options specified if they were listed in the options.

So you cant make a switch() use default: to complain of an unknown option. :(

  

#

[Official documentation page](https://www.php.net/manual/en/function.getopt.php)

**[To root](/README.md)**