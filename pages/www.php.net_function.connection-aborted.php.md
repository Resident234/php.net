# connection_aborted



A trick to detecting if a connection is closed without having to send data that will otherwise corrupt the stream of data (like a binary file) you can use a combination of chunking the data on HTTP/1.1 by sending a "0" ("zero") as a leading chunk size without anything else.<br><br>*NOTE* it&apos;s important to note that it&apos;s not a good idea to check the stream more then once every few seconds. By doing this you are potentially increasing the data sent to the user with no gain to the user.<br><br>A good reason to do it this way is if you are generating a report that takes a long time to run and takes a lot of server resources. This would allow the server to detect if a user canceled the download and do any cleanup without corrupting the file file being download.<br><br>Here is an example:<br><br>

```
<?php
ignore_user_abort(true);
header(&apos;Transfer-Encoding:chunked&apos;);
ob_flush();
flush();
$start = microtime(true);
$i = 0;
// Use this function to echo anything to the browser.
function vPrint($data){
    if(strlen($data))
        echo dechex(strlen($data)), "\r\n", $data, "\r\n";
    ob_flush();
    flush();
}
// You MUST execute this function after you are done streaming information to the browser.
function endPacket(){
    echo "0\r\n\r\n";
    ob_flush();
    flush();
}
do{
    echo "0";
    ob_flush();
    flush();
    if(connection_aborted()){
        // This happens when connection is closed
        file_put_contents(&apos;/tmp/test.tmp&apos;, sprintf("Conn Closed\nTime spent with connection open: %01.5f sec\nLoop itterations: %s\n\n", microtime(true) - $start, $i), FILE_APPEND);
        endPacket();
        exit;
    }
    usleep(50000);
    vPrint("I get echo&apos;ed every itteration (every .5 second)&lt;br /&gt;\n");
}while($i++ &lt; 200);
endPacket();
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.connection-aborted.php)

**[To root](/README.md)**