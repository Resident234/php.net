# Errors in PHP 7





You can catch both exceptions and errors by catching(Throwable)

  

#



Throwable does not work on PHP 5.x.

To catch both exceptions and errors in PHP 5.x and 7, add a catch block for Exception AFTER catching Throwable first.
Once PHP 5.x support is no longer needed, the block catching Exception can be removed.

try
{
&#xA0;&#xA0; // Code that may throw an Exception or Error.
}
catch (Throwable $t)
{
&#xA0;&#xA0; // Executed only in PHP 7, will not match in PHP 5
}
catch (Exception $e)
{
&#xA0;&#xA0; // Executed only in PHP 5, will not be reached in PHP 7
}

  

#

[Official documentation page](https://www.php.net/manual/en/language.errors.php7.php)

**[To root](/README.md)**