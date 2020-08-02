# The ArrayObject class



As you know ArrayObject is not an array so you can&apos;t use the built in array functions. Here&apos;s a trick around that:<br><br>Extend the ArrayObject class with your own and implement this magic method:<br><br>

```
<?php
    public function __call($func, $argv)
    {
        if (!is_callable($func) || substr($func, 0, 6) !== 'array_')
        {
            throw new BadMethodCallException(__CLASS__.'->'.$func);
        }
        return call_user_func_array($func, array_merge(array($this->getArrayCopy()), $argv));
    }
?>
```


Now you can do this with any array_* function:


```
<?php
$yourObject->array_keys();
?>
```
<br>- Don&apos;t forget to ommit the first parameter - it&apos;s automatic!<br><br>Note: You might want to write your own functions if you&apos;re working with large sets of data.  

---

There is a better explanation about the ArrayObject flags (STD_PROP_LIST and ARRAY_AS_PROPS) right here: <br><br>http://stackoverflow.com/a/16619183/1019305<br><br>Thanks to JayTaph  

---

I found the description of STD_PROP_LIST a bit vague, so I put together a simple demonstration to show its behavior:<br><br>

```
<?php                                                     
                                                          
$a = new ArrayObject(array(), ArrayObject::STD_PROP_LIST);
    $a['arr'] = 'array data';                             
    $a->prop = 'prop data';                               
$b = new ArrayObject();                                   
    $b['arr'] = 'array data';                             
    $b->prop = 'prop data';                               
                                                          
// ArrayObject Object                                     
// (                                                      
//      [prop] => prop data                               
// )                                                      
print_r($a);                                              
                                                          
// ArrayObject Object                                     
// (                                                      
//      [arr] => array data                               
// )                                                      
print_r($b);                                              
                                                          
?>
```
  

---

I don&apos;t believe the same performance is true since PHP 5.3. Using the same fill, read_key and foreach approach on both native arrays and ArrayObjects with 10000 keys I get the following<br><br>PHP 5.2<br><br>array() fill         0.013101<br>array() read         0.008685<br>array() foreach      0.004319<br>ArrayObject fill     0.014136<br>ArrayObject read     0.010003<br>ArrayObject foreach  3.454612<br><br>PHP 5.3<br><br>array() fill         0.010395<br>array() read         0.005933<br>array() foreach      0.001903<br>ArrayObject fill     0.010598<br>ArrayObject read     0.006387<br>ArrayObject foreach  0.003451<br><br>This was the code I used for both, an array or ArrayObject is passed into each of the functions. Again PEAR::Benchmark was used to get the results.<br><br>

```
<?php
require_once 'Benchmark/Timer.php';

define('KEYS', 10000);

function fill(&amp;$arr) {
    for ($i = 1; $i <= KEYS; $i++) {
        $arr['key-' . $i] = $i;
    }
}

function read_key(&amp;$arr) {
    for ($i = 1; $i <= KEYS; $i++) {
        $v = $arr['key-' . $i];
    }
}

function fe(&amp;$arr) {
    foreach ($arr as $key => $value) {
        $v = $value;
    }
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.arrayobject.php)

**[To root](/README.md)**