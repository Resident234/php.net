# Object Iteration



there is still an open bug about using current() etc. with iterators<br>https://bugs.php.net/bug.php?id=49369  

#

By reading the posts below I wondered if it really is impossible to make an ArrayAccess implementation really behave like a true array ( by being multi level )<br><br>Seems like it&apos;s not impossible. Not very preety but usable<br><br>

```
<?php

class ArrayAccessImpl implements ArrayAccess {

  private $data = array();

  public function offsetUnset($index) {}

  public function offsetSet($index, $value) {
//    echo ("SET: ".$index."&lt;br&gt;");
    
    if(isset($data[$index])) {
        unset($data[$index]);
    }
    
    $u = &amp;$this->data[$index];
    if(is_array($value)) {
        $u = new ArrayAccessImpl();
        foreach($value as $idx=>$e)
            $u[$idx]=$e;
    } else
        $u=$value;
  }

  public function offsetGet($index) {
//    echo ("GET: ".$index."&lt;br&gt;");

    if(!isset($this->data[$index]))
        $this->data[$index]=new ArrayAccessImpl();
    
    return $this->data[$index];
  }

  public function offsetExists($index) {
//    echo ("EXISTS: ".$index."&lt;br&gt;");
    
    if(isset($this->data[$index])) {
        if($this->data[$index] instanceof ArrayAccessImpl) {
            if(count($this->data[$index]->data)&gt;0)
                return true;
            else
                return false;
        } else
            return true;
    } else
        return false;
  }

}

echo "ArrayAccess implementation that behaves like a multi-level array&lt;hr /&gt;";

$data = new ArrayAccessImpl();

$data['string']="Just a simple string";
$data['number']=33;
$data['array']['another_string']="Alpha";
$data['array']['some_object']=new stdClass();
$data['array']['another_array']['x']['y']="LOL @ Whoever said it can't be done !";
$data['blank_array']=array();

echo "'array' Isset? "; print_r(isset($data['array'])); echo "&lt;hr /&gt;";
echo "&lt;pre&gt;"; print_r($data['array']['non_existent']); echo "&lt;/pre&gt;If attempting to read an offset that doesn't exist it returns a blank object! Use isset() to check if it exists!&lt;br&gt;";
echo "'non_existent' Isset? "; print_r(isset($data['array']['non_existent'])); echo "&lt;br /&gt;";
echo "&lt;pre&gt;"; print_r($data['blank_array']); echo "&lt;/pre&gt;A blank array unfortunately returns similar results :(&lt;br /&gt;";
echo "'blank_array' Isset? "; print_r(isset($data['blank_array'])); echo "&lt;hr /&gt;";
echo "&lt;pre&gt;"; print_r($data); echo "&lt;/pre&gt; (non_existent remains in the structure. If someone can help to solve this I'll appreciate it)&lt;hr /&gt;";

echo "Display some value that exists: ".$data['array']['another_string'];

?>
```
<br><br>(in the two links mentioned below by artur at jedlinski... they say you can&apos;t use references, so I didn&apos;t used them.<br>My implementation uses recursive objects)<br><br>If anyone finds a better (cleaner) sollution, please e-mail me.<br>Thanks,<br>Wave.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.iterations.php)

**[To root](/README.md)**