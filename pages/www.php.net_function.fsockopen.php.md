# fsockopen



just a quick note for users attempting https and thinking they must resort to curl or alternate methods -<br>you can use fsockopen, just read the docs closely.  basically they are saying to use &apos;ssl://&apos; for a HTTPS (SSL) web request.<br><br>so this would work for authorize.net, and others; even for that paypal IPN - however I think it would be best to leave the site and deal with paypal&apos;s form:<br><br>

```
<?php
$host = "something.example.com";
$port = 443;
$path = "/the/url/path/file.php"; //or .dll, etc. for authnet, etc.

//you will need to setup an array of fields to post with
//then create the post string
$formdata = array ( "x_field" => "somevalue");
//build the post string
  foreach($formdata AS $key => $val){
    $poststring .= urlencode($key) . "=" . urlencode($val) . "&amp;";
  }
// strip off trailing ampersand
$poststring = substr($poststring, 0, -1);

$fp = fsockopen("ssl://".$host, $port, $errno, $errstr, $timeout = 30);

if(!$fp){
 //error tell us
 echo "$errstr ($errno)\n";
   
}else{

  //send the server request
  fputs($fp, "POST $path HTTP/1.1\r\n");
  fputs($fp, "Host: $host\r\n");
  fputs($fp, "Content-type: application/x-www-form-urlencoded\r\n");
  fputs($fp, "Content-length: ".strlen($poststring)."\r\n");
  fputs($fp, "Connection: close\r\n\r\n");
  fputs($fp, $poststring . "\r\n\r\n");

  //loop through the response from the server
  while(!feof($fp)) {
    echo fgets($fp, 4096);
  }
  //close fp - we are done with it
  fclose($fp);
}
?>
```
  

#



```
<?php
// This script is an example of posting multiple files using 
// fsockopen.
// The tricky part is making sure the HTTP headers and file boundaries are acceptable to the target webserver.
// This script is for example purposes only and could/should be improved upon.

$host='targethost';
$port=80;
$path='/test/socket/file_upload/receive_files.php';

// the file you want to upload 
$file_array[0] = "dingoboy.gif"; // the file 
$file_array[1] = "dingoboy2.gif"; // the file 
$file_array[2] = "dingoboy3.gif"; // the file 
$content_type = "image/gif"; // the file mime type
//$content_type = "text/plain";
//echo "file_array[0]:$file_array[0]<br><br>";

srand((double)microtime()*1000000);
$boundary = "---------------------------".substr(md5(rand(0,32000)),0,10);

$data = "--$boundary";

for($i=0;$i<count($file_array);$i++){
   $content_file = join("", file($file_array[$i]));

   $data.="
Content-Disposition: form-data; name=\"file".($i+1)."\"; filename=\"$file_array[$i]\"
Content-Type: $content_type 

$content_file
--$boundary";

}

$data.="--\r\n\r\n";

$msg =
"POST $path HTTP/1.0
Content-Type: multipart/form-data; boundary=$boundary
Content-Length: ".strlen($data)."\r\n\r\n";

$result="";

// open the connection
$f = fsockopen($host, $port);

fputs($f,$msg.$data);

// get the response
while (!feof($f)) $result .= fread($f,32000);

fclose($f);

?>
```
  

#

This script checks specific ports so you need to have the correct port open on the server for this to work.<br><br>E.g if i have a windows domain controller and it is servering LDAP then the following would be used to check it is online:<br>

```
<?php
chkServer("MyDC", "389");
?>
```


for a webserver:


```
<?php
chkServer("MyWebSvr", "80");
?>
```


etc etc
--------------------------------------------------------



```
<?php
// check if a server is up by connecting to a port
function chkServer($host, $port)
{   
    $hostip = @gethostbyname($host); // resloves IP from Hostname returns hostname on failure
    
    if ($hostip == $host) // if the IP is not resloved
    {
        echo "Server is down or does not exist";
    }
    else
    {
        if (!$x = @fsockopen($hostip, $port, $errno, $errstr, 5)) // attempt to connect
        {
            echo "Server is down";
        }
        else 
        {
            echo "Server is up";
            if ($x)
            {
                @fclose($x); //close connection
            }
        }  
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.fsockopen.php)

**[To root](/README.md)**