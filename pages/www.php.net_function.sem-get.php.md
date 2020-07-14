# sem_get



Actually it looks like the semaphore is automatically released not on request shutdown but when the variable you store it&apos;s resource ID is freed. That is a very big difference.  

#

[Official documentation page](https://www.php.net/manual/en/function.sem-get.php)

**[To root](/README.md)**