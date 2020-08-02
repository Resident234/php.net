# The SplFileObject class



Note that this class has a private (and thus, not documented) property that holds the file pointer. Combine this with the fact that there is no method to close the file handle, and you get into situations where you are not able to delete the file with unlink(), etc., because an SplFileObject still has a handle open.<br><br>To get around this issue, delete the SplFileObject like this:<br><br>---------------------------------------------------------------------<br>

```
<?php
print "Declaring file object\n";
$file = new SplFileObject('example.txt');

print "Trying to delete file...\n";
unlink('example.txt');

print "Closing file object\n";
$file = null;

print "Deleting file...\n";
unlink('example.txt');

print 'File deleted!';
?>
```
<br>---------------------------------------------------------------------<br><br>which will output:<br><br>---------------------------------------------------------------------<br>Declaring file object <br>Trying to delete file... <br><br>Warning: unlink(example.txt): Permission denied in file.php on line 6<br>Closing file object <br>Deleting file... <br>File deleted!<br>---------------------------------------------------------------------  

---

[Official documentation page](https://www.php.net/manual/en/class.splfileobject.php)

**[To root](/README.md)**