# filetype





There are 7 values that can be returned. Here is a list of them and what each one means

block: block special device

char: character special device

dir: directory

fifo: FIFO (named pipe)

file: regular file

link: symbolic link

unknown: unknown file type

  

#



filetype() does not work for files &gt;=2GB on x86 Linux. You can use stat as a workarround:

$type=trim(`stat -c%F $file`);

Note that stat returns diffenerent strings (&quot;regular file&quot;,&quot;directory&quot;,...)

  

#

[Official documentation page](https://www.php.net/manual/en/function.filetype.php)

**[To root](/README.md)**