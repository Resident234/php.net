# ReflectionMethod::invokeArgs



We can do black magic, which is useful in templating block calls:<br><br>

```
<?php
     $object->__named('methodNameHere', array('arg3' => 'three', 'arg1' => 'one'));

     ...

      /**
       * Pass method arguments by name
       *
       * @param string $method
       * @param array $args
       * @return mixed
       */
      public function __named($method, array $args = array())
      {
        $reflection = new ReflectionMethod($this, $method);

        $pass = array();
        foreach($reflection->getParameters() as $param)
        {
          /* @var $param ReflectionParameter */
          if(isset($args[$param->getName()]))
          {
            $pass[] = $args[$param->getName()];
          }
          else
          {
            $pass[] = $param->getDefaultValue();
          }
        }

        return $reflection->invokeArgs($this, $pass);
      }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.invokeargs.php)

**[To root](/README.md)**