# Extending Exceptions



As previously noted exception linking was recently added (and what a god-send it is, it certainly makes layer abstraction (and, by association, exception tracking) easier).<br><br>Since &lt;5.3 was lacking this useful feature I took some initiative and creating a custom exception class that all of my exceptions inherit from:<br><br>

```
<?php

class SystemException extends Exception
{
    private $previous;
    
    public function __construct($message, $code = 0, Exception $previous = null)
    {
        parent::__construct($message, $code);
        
        if (!is_null($previous))
        {
            $this -&gt; previous = $previous;
        }
    }
    
    public function getPrevious()
    {
        return $this -&gt; previous;
    }
}

?>
```
<br><br>Hope you find it useful.  

#

[Official documentation page](https://www.php.net/manual/en/language.exceptions.extending.php)

**[To root](/README.md)**