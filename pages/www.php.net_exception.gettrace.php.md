# Exception::getTrace



Two important points about this function which are not documented:<br><br>1) The trace does not include the file / line at which the exception is thrown; that entry is only recorded in the top-level getFile/Line methods.<br><br>2) Elements are returned in &apos;closest-first&apos; order, e.g. if you have a script x which calls function y which calls function z which throws an exception, then the first trace element will be &apos;Y&apos; and the second will be &apos;X&apos;.  

#

[Official documentation page](https://www.php.net/manual/en/exception.gettrace.php)

**[To root](/README.md)**