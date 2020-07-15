# md5_file



If you just need to find out if two files are identical, comparing file hashes can be inefficient, especially on large files.  There&apos;s no reason to read two whole files and do all the math if the second byte of each file is different.  If you don&apos;t need to store the hash value for later use, there may not be a need to calculate the hash value just to compare files.  This can be much faster:<br><br>

```
<?php
define('READ_LEN', 4096);

if(files_identical('file1.txt', 'file2.txt'))
    echo 'files identical';
else
    echo 'files not identical';

//   pass two file names
//   returns TRUE if files are the same, FALSE otherwise
function files_identical($fn1, $fn2) {
    if(filetype($fn1) !== filetype($fn2))
        return FALSE;

    if(filesize($fn1) !== filesize($fn2))
        return FALSE;

    if(!$fp1 = fopen($fn1, 'rb'))
        return FALSE;

    if(!$fp2 = fopen($fn2, 'rb')) {
        fclose($fp1);
        return FALSE;
    }

    $same = TRUE;
    while (!feof($fp1) and !feof($fp2))
        if(fread($fp1, READ_LEN) !== fread($fp2, READ_LEN)) {
            $same = FALSE;
            break;
        }

    if(feof($fp1) !== feof($fp2))
        $same = FALSE;

    fclose($fp1);
    fclose($fp2);

    return $same;
}
?>
```
  

#

It&apos;s faster to use md5sum than openssl md5:<br><br>

```
<?php
$begin = microtime(true);

$file_path = '../backup_file1.tar.gz';
$result = explode("  ", exec("md5sum $file_path"));
echo "Hash = ".$result[0]."&lt;br /&gt;";

# Here 7 other big files (20-300 MB)

$end = microtime(true) - $begin;
echo "Time = $end";
# Time = 4.4475841522217 

#Method with openssl
# Time = 12.1463856900543
?>
```
<br><br>About 3x faster  

#

[Official documentation page](https://www.php.net/manual/en/function.md5-file.php)

**[To root](/README.md)**