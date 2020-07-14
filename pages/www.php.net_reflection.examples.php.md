# Examples



If you want to use method closures and don&apos;t have PHP 5.3, perhaps you find useful the following function<br>

```
<?php
    function get_method_closure($object,$method_name){
        if(method_exists(get_class($object),$method_name)){
            $func            = create_function( &apos;&apos;,
                            &apos;
                                $args            = func_get_args();
                                static $object    = NULL;
                                
                                /*
                                * Check if this function is being called to set the static $object, which 
                                * containts scope information to invoke the method
                                */
                                if(is_null($object) &amp;&amp; count($args) &amp;&amp; get_class($args[0])=="&apos;.get_class($object).&apos;"){
                                    $object = $args[0];
                                    return;
                                }

                                if(!is_null($object)){
                                    return call_user_func_array(array($object,"&apos;.$method_name.&apos;"),$args);
                                }else{
                                    return FALSE;
                                }&apos;
                            );
            
            //Initialize static $object
            $func($object);
            
            //Return closure
            return $func;
        }else{
            return false;
        }        
    }
?>
```

Here is how you would use it:


```
<?php
class foo{
    public function bar($foo){
        return strtolower($foo);
    }
}

$foo = new foo();
$f = get_method_closure($foo,&apos;bar&apos;);
echo $f("BAR");//Prints &apos;bar&apos;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflection.examples.php)

**[To root](/README.md)**