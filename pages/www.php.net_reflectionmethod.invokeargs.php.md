# ReflectionMethod::invokeArgs



We can do black magic, which is useful in templating block calls:<br><br>

```
<?php
     $object-&gt;__named(&apos;methodNameHere&apos;, array(&apos;arg3&apos; =&gt; &apos;three&apos;, &apos;arg1&apos; =&gt; &apos;one&apos;));

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
        foreach($reflection-&gt;getParameters() as $param)
        {
          /* @var $param ReflectionParameter */
          if(isset($args[$param-&gt;getName()]))
          {
            $pass[] = $args[$param-&gt;getName()];
          }
          else
          {
            $pass[] = $param-&gt;getDefaultValue();
          }
        }

        return $reflection-&gt;invokeArgs($this, $pass);
      }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.invokeargs.php)

**[To root](/README.md)**