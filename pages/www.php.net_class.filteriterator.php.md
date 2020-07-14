# The FilterIterator class



The code below is a simple example of usage . Note that the method which does the actual job is accept. <br><br>

```
<?php
class UserFilter extends FilterIterator 
{
    private $userFilter;
    
    public function __construct(Iterator $iterator , $filter )
    {
        parent::__construct($iterator);
        $this-&gt;userFilter = $filter;
    }
    
    public function accept()
    {
        $user = $this-&gt;getInnerIterator()-&gt;current();
        if( strcasecmp($user[&apos;name&apos;],$this-&gt;userFilter) == 0) {
            return false;
        }        
        return true;
    }
}

$array = array(
array(&apos;name&apos; =&gt; &apos;Jonathan&apos;,&apos;id&apos; =&gt; &apos;5&apos;),
array(&apos;name&apos; =&gt; &apos;Abdul&apos; ,&apos;id&apos; =&gt; &apos;22&apos;)
);

$object = new ArrayObject($array);

// Note it is case insensitive check in our example due the usage of strcasecmp function
$iterator = new UserFilter($object-&gt;getIterator(),&apos;abdul&apos;);

foreach ($iterator as $result) {
    echo $result[&apos;name&apos;];
}

/* Outputs Jonathan */

?>
```
<br>Regards.  

#

[Official documentation page](https://www.php.net/manual/en/class.filteriterator.php)

**[To root](/README.md)**