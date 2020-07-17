# ftp_rawlist



Here&apos;s a simple function that&apos;ll parse the data returned by ftp_rawlist() into an associative array. I wrote it because some of the functions listed below are way to long, complex or won&apos;t work with file names that contain spaces.<br><br>

```
<?php
    function listDetailed($resource, $directory = '.') {
        if (is_array($children = @ftp_rawlist($resource, $directory))) {
            $items = array();

            foreach ($children as $child) {
                $chunks = preg_split("/\s+/", $child);
                list($item['rights'], $item['number'], $item['user'], $item['group'], $item['size'], $item['month'], $item['day'], $item['time']) = $chunks;
                $item['type'] = $chunks[0]{0} === 'd' ? 'directory' : 'file';
                array_splice($chunks, 0, 8);
                $items[implode(" ", $chunks)] = $item;
            }

            return $items;
        }

        // Throw exception or return false < up to you
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-rawlist.php)

**[To root](/README.md)**