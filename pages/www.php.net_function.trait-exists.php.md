# trait_exists





```
<?php<br>trait World {<br><br>    private static $instance;<br>    protected $tmp;<br><br>    public static function World()<br>    {<br>        self::$instance = new static();<br>        self::$instance-&gt;tmp = get_called_class().&apos; &apos;.__TRAIT__;<br>        <br>        return self::$instance;<br>    }<br><br>}<br><br>if ( trait_exists( &apos;World&apos; ) ) {<br>    <br>    class Hello {<br>        use World;<br><br>        public function text( $str )<br>        {<br>            return $this-&gt;tmp.$str;<br>        }<br>    }<br><br>}<br><br>echo Hello::World()-&gt;text(&apos;!!!&apos;); // Hello World!!!  

#

[Official documentation page](https://www.php.net/manual/en/function.trait-exists.php)

**[To root](/README.md)**