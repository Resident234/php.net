# The Serializable interface





Here&apos;s an example how to un-, serialize more than one property:

class Example implements \Serializable
{
&#xA0; &#xA0; protected $property1;
&#xA0; &#xA0; protected $property2;
&#xA0; &#xA0; protected $property3;

&#xA0; &#xA0; public function __construct($property1, $property2, $property3)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property1 = $property1;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property2 = $property2;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property3 = $property3;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function serialize()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return serialize([
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property1,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property2,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property3,
&#xA0; &#xA0; &#xA0; &#xA0; ]);
&#xA0; &#xA0; }

&#xA0; &#xA0; public function unserialize($data)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; list(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property1,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property2,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;property3
&#xA0; &#xA0; &#xA0; &#xA0; ) = unserialize($data);
&#xA0; &#xA0; }

}

  

#

[Official documentation page](https://www.php.net/manual/en/class.serializable.php)

**[To root](/README.md)**