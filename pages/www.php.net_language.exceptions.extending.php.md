# Extending Exceptions





As previously noted exception linking was recently added (and what a god-send it is, it certainly makes layer abstraction (and, by association, exception tracking) easier).

Since &lt;5.3 was lacking this useful feature I took some initiative and creating a custom exception class that all of my exceptions inherit from:



```
<?php

class SystemException extends Exception
{
&#xA0; &#xA0; private $previous;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __construct($message, $code = 0, Exception $previous = null)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct($message, $code);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if (!is_null($previous))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this -&gt; previous = $previous;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getPrevious()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return $this -&gt; previous;
&#xA0; &#xA0; }
}

?>
```


Hope you find it useful.

  

#

[Official documentation page](https://www.php.net/manual/en/language.exceptions.extending.php)

**[To root](/README.md)**