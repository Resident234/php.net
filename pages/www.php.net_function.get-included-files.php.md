# get_included_files





As of PHP5, this function seems to return an array with the first index being the script all subsequent scripts are included to.
If index.php includes b.php and c.php and calls get_included_files(), the returned array looks as follows:

index.php
a.php
b.php

while in PHP&lt;5 the array would be:

a.php
b.php

If you want to know which is the script that is including current script you can use $_SERVER[&apos;SCRIPT_FILENAME&apos;] or any other similar server global.

If you also want to ensure current script is being included and not run independently you should evaluate following expression:

__FILE__ != $_SERVER[&apos;SCRIPT_FILENAME&apos;]

If this expression returns TRUE, current script is being included or required.

  

#

[Official documentation page](https://www.php.net/manual/en/function.get-included-files.php)

**[To root](/README.md)**