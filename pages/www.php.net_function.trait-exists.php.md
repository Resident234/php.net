# trait_exists





```
<?php
trait World {

    private static $instance;
    protected $tmp;

    public static function World()
    {
        self::$instance = new static();
        self::$instance->tmp = get_called_class().' '.__TRAIT__;
        
        return self::$instance;
    }

}

if ( trait_exists( 'World' ) ) {
    
    class Hello {
        use World;

        public function text( $str )
        {
            return $this->tmp.$str;
        }
    }

}

echo Hello::World()->text('!!!'); // Hello World!!!?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.trait-exists.php)

**[To root](/README.md)**