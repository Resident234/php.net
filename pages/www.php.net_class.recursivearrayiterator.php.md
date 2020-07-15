# The RecursiveArrayIterator class



If you are iterating over a multi-dimensional array of objects, you may be tempted to use a RecursiveArrayIterator within a RecursiveIteratorIterator. You are likely to get baffling results if you do. That is because RecursiveArrayIterator treats all objects as having children, and tries to recurse into them. But if you are interested in having your RecursiveIteratorIterator return the objects in your multi-dimensional array, then you don&apos;t want the default setting LEAVES_ONLY, because no object can be a leaf (= has no children).<br><br>The solution is to extend the RecursiveArrayIterator class and override the hasChildren method appropriately. Something like the following might be suitable:<br><br>

```
<?php
class RecursiveArrayOnlyIterator extends RecursiveArrayIterator {
  public function hasChildren() {
    return is_array($this->current());
  }
}
?>
```
<br>Of course, this simple example will not recurse into ArrayObjects either!  

#

Using the RecursiveArrayIterator to traverse an unknown amount of sub arrays within the outer array. Note: This functionality is already provided by using the RecursiveIteratorIterator but is useful in understanding how to use the iterator when using for the first time as all the terminology does get rather confusing at first sight of SPL!<br><br>

```
<?php
$myArray = array(
    0 => 'a',
    1 => array('subA','subB',array(0 => 'subsubA', 1 => 'subsubB', 2 => array(0 => 'deepA', 1 => 'deepB'))),
    2 => 'b',
    3 => array('subA','subB','subC'),
    4 => 'c'
);

$iterator = new RecursiveArrayIterator($myArray);
iterator_apply($iterator, 'traverseStructure', array($iterator));

function traverseStructure($iterator) {
    
    while ( $iterator -> valid() ) {

        if ( $iterator -> hasChildren() ) {
        
            traverseStructure($iterator -> getChildren());
            
        }
        else {
            echo $iterator -> key() . ' : ' . $iterator -> current() .PHP_EOL;    
        }

        $iterator -> next();
    }
}
?>
```
<br><br>The output from which is:<br>0 : a<br>0 : subA<br>1 : subB<br>0 : subsubA<br>1 : subsubB<br>0 : deepA<br>1 : deepB<br>2 : b<br>0 : subA<br>1 : subB<br>2 : subC<br>4 : c  

#

[Official documentation page](https://www.php.net/manual/en/class.recursivearrayiterator.php)

**[To root](/README.md)**