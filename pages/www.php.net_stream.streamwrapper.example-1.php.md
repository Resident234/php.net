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
        $url['path'] = substr($url['path'], 1);
        try{
            $this->_pdo = new PDO("mysql:host={$url['host']};dbname={$url['path']}", $url['user'], isset($url['pass'])? $url['pass'] : '', array());
        } catch(PDOException $e){ return false; }
        switch ($mode){
            case 'w' : 
                $this->_ps = $this->_pdo->prepare('INSERT INTO data VALUES(null, ?, NOW())');
                break;
            case 'r' : 
                $this->_ps = $this->_pdo->prepare('SELECT id, data FROM data WHERE id > ? LIMIT 1');
                break;
            default  : return false;
        }
        return true;
    }

    function stream_read()
    {
         $this->_ps->execute(array($this->_rowId));
         if($this->_ps->rowCount() == 0) return false;
         $this->_ps->bindcolumn(1, $this->_rowId);
         $this->_ps->bindcolumn(2, $ret);
         $this->_ps->fetch();
         return $ret;
    }

    function stream_write($data)
    {
        $this->_ps->execute(array($data));
        return strlen($data);
    }

    function stream_tell()
    {
        return $this->_rowId;
    }

    function stream_eof()
    {
        $this->_ps->execute(array($this->_rowId));
        return (bool) $this->_ps->rowCount();
    }

    function stream_seek($offset, $step)
    {
        //No need to be implemented
    }
}

stream_register_wrapper('db', 'DBStream');

$fr = fopen('db://testuser@localhost/testdb', 'r');
$fw = fopen('db://testuser:testpassword@localhost/testdb', 'w');
//The two forms above are accepted : for the former, the default password "" will be used

$alg = hash_algos();
$al = $alg[array_rand($alg)];
$data = hash($al, rand(rand(0, 9), rand(10, 999))); // Some random data to be written
fwrite($fw, $data); // Writing the data to the wrapper
while($a = fread($fr, 256)){ //A loop for reading from the wrapper
    echo $a . '<br />';
}
?>
```
<br><br>Hope it helps, cheers ;)  

#

[Official documentation page](https://www.php.net/manual/en/stream.streamwrapper.example-1.php)

**[To root](/README.md)**