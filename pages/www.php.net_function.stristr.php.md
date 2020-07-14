# stristr



There was a change in PHP 4.2.3 that can cause a warning message<br>to be generated when using stristr(), even though no message was<br>generated in older versions of PHP.<br><br>The following will generate a warning message in 4.0.6 and 4.2.3:<br>  stristr("haystack", "");<br>     OR<br>  $needle = "";  stristr("haystack", $needle);<br><br>This will _not_ generate an "Empty Delimiter" warning message in<br>4.0.6, but _will_ in 4.2.3:<br>  unset($needle); stristr("haystack", $needle);<br><br>Here&apos;s a URL that documents what was changed:<br>http://groups.google.ca/groups?selm=cvshholzgra1031224321%40cvsserver  

#

[Official documentation page](https://www.php.net/manual/en/function.stristr.php)

**[To root](/README.md)**