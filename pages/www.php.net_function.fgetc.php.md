# fgetc



The best and simplest way to get input from a user in the CLI with only PHP is to use fgetc() function with the STDIN constant:<br><br>

```
<?php

echo &apos;Are you sure you want to quit? (y/n) &apos;;
$input = fgetc(STDIN);

if ($input == &apos;y&apos;)
{
    exit(0);
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.fgetc.php)

**[To root](/README.md)**