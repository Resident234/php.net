# The Serializable interface




<div class="phpcode"><span class="html">
Here&apos;s an example how to un-, serialize more than one property:<br><br>class Example implements \Serializable<br>{<br>&#xA0; &#xA0; protected $property1;<br>&#xA0; &#xA0; protected $property2;<br>&#xA0; &#xA0; protected $property3;<br><br>&#xA0; &#xA0; public function __construct($property1, $property2, $property3)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property1 = $property1;<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property2 = $property2;<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property3 = $property3;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function serialize()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return serialize([<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property1,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property2,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property3,<br>&#xA0; &#xA0; &#xA0; &#xA0; ]);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function unserialize($data)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; list(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property1,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property2,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property3<br>&#xA0; &#xA0; &#xA0; &#xA0; ) = unserialize($data);<br>&#xA0; &#xA0; }<br><br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.serializable.php)

**[To root](/README.md)**