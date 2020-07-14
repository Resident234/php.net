# Example class registered as stream wrapper



Example of stream used with databases :<br><br>Requirement :<br>A MySQL database with a table named data structured as follow :<br>CREATE TABLE IF NOT EXISTS `data` (<br>  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,<br>  `data` varchar(255) NOT NULL,<br>  `when_inserted` datetime NOT NULL,<br>  PRIMARY KEY (`id`)<br>) ENGINE=MyISAM  DEFAULT CHARSET=latin1 AUTO_INCREMENT=1 ;<br><br>Now with the stream implementation :<br><br>

```
<?php

class DBStream {
    private $_pdo;
    private $_ps;
    private $_rowId = 0;

    function stream_open($path, $mode, $options, &amp;$opath)
    {
        $url = parse_url($path);
        $url[&apos;path&apos;] = substr($url[&apos;path&apos;], 1);
        try{
            $this-&gt;_pdo = new PDO("mysql:host={$url[&apos;host&apos;]};dbname={$url[&apos;path&apos;]}", $url[&apos;user&apos;], isset($url[&apos;pass&apos;])? $url[&apos;pass&apos;] : &apos;&apos;, array());
        } catch(PDOException $e){ return false; }
        switch ($mode){
            case &apos;w&apos; : 
                $this-&gt;_ps = $this-&gt;_pdo-&gt;prepare(&apos;INSERT INTO data VALUES(null, ?, NOW())&apos;);
                break;
            case &apos;r&apos; : 
                $this-&gt;_ps = $this-&gt;_pdo-&gt;prepare(&apos;SELECT id, data FROM data WHERE id &gt; ? LIMIT 1&apos;);
                break;
            default  : return false;
        }
        return true;
    }

    function stream_read()
    {
         $this-&gt;_ps-&gt;execute(array($this-&gt;_rowId));
         if($this-&gt;_ps-&gt;rowCount() == 0) return false;
         $this-&gt;_ps-&gt;bindcolumn(1, $this-&gt;_rowId);
         $this-&gt;_ps-&gt;bindcolumn(2, $ret);
         $this-&gt;_ps-&gt;fetch();
         return $ret;
    }

    function stream_write($data)
    {
        $this-&gt;_ps-&gt;execute(array($data));
        return strlen($data);
    }

    function stream_tell()
    {
        return $this-&gt;_rowId;
    }

    function stream_eof()
    {
        $this-&gt;_ps-&gt;execute(array($this-&gt;_rowId));
        return (bool) $this-&gt;_ps-&gt;rowCount();
    }

    function stream_seek($offset, $step)
    {
        //No need to be implemented
    }
}

stream_register_wrapper(&apos;db&apos;, &apos;DBStream&apos;);

$fr = fopen(&apos;db://testuser@localhost/testdb&apos;, &apos;r&apos;);
$fw = fopen(&apos;db://testuser:testpassword@localhost/testdb&apos;, &apos;w&apos;);
//The two forms above are accepted : for the former, the default password "" will be used

$alg = hash_algos();
$al = $alg[array_rand($alg)];
$data = hash($al, rand(rand(0, 9), rand(10, 999))); // Some random data to be written
fwrite($fw, $data); // Writing the data to the wrapper
while($a = fread($fr, 256)){ //A loop for reading from the wrapper
    echo $a . &apos;&lt;br /&gt;&apos;;
}
?>
```
<br><br>Hope it helps, cheers ;)  

#

[Official documentation page](https://www.php.net/manual/en/stream.streamwrapper.example-1.php)

**[To root](/README.md)**