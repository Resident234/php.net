# stream_set_blocking



On Windows this function does not work with pipes opened with proc_open (https://bugs.php.net/bug.php?id=47918, https://bugs.php.net/bug.php?id=34972, https://bugs.php.net/bug.php?id=51800)  

---

When you use fwrite() on a non-blocking stream, data isn&apos;t discarded silently as t dot starling said.<br><br>Remember that fwrite() returns an int, and this int represents the amount of data really written to the stream. So, if you see that fwrite() returns less than the amount of written data, it means you&apos;ll have to call fwrite() again in the future to write the remaining amount of data.<br><br>You can use stream_select() to wait for the stream to be available for writing, then continue writing data to the stream.<br><br>Non-blocking streams are useful as you can have more than one non-blocking stream, and wait for them to be available for writing.  

---

[Official documentation page](https://www.php.net/manual/en/function.stream-set-blocking.php)

**[To root](/README.md)**