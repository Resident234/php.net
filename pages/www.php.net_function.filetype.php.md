# filetype



There are 7 values that can be returned. Here is a list of them and what each one means<br><br>block: block special device<br><br>char: character special device<br><br>dir: directory<br><br>fifo: FIFO (named pipe)<br><br>file: regular file<br><br>link: symbolic link<br><br>unknown: unknown file type  

#

filetype() does not work for files &gt;=2GB on x86 Linux. You can use stat as a workarround:<br><br>$type=trim(`stat -c%F $file`);<br><br>Note that stat returns diffenerent strings ("regular file","directory",...)  

#

[Official documentation page](https://www.php.net/manual/en/function.filetype.php)

**[To root](/README.md)**