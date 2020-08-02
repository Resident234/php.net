# The Serializable interface



Here&apos;s an example how to un-, serialize more than one property:<br><br>class Example implements \Serializable<br>{<br>    protected $property1;<br>    protected $property2;<br>    protected $property3;<br><br>    public function __construct($property1, $property2, $property3)<br>    {<br>        $this-&gt;property1 = $property1;<br>        $this-&gt;property2 = $property2;<br>        $this-&gt;property3 = $property3;<br>    }<br><br>    public function serialize()<br>    {<br>        return serialize([<br>            $this-&gt;property1,<br>            $this-&gt;property2,<br>            $this-&gt;property3,<br>        ]);<br>    }<br><br>    public function unserialize($data)<br>    {<br>        list(<br>            $this-&gt;property1,<br>            $this-&gt;property2,<br>            $this-&gt;property3<br>        ) = unserialize($data);<br>    }<br><br>}  

---

[Official documentation page](https://www.php.net/manual/en/class.serializable.php)

**[To root](/README.md)**