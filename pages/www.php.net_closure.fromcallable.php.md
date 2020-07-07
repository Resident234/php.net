# Closure::fromCallable





It seems that the result of the &quot;fromCallable&quot; behaves a little bit different then an original Lambda function.

class A {
&#xA0; &#xA0; private $name;
&#xA0; &#xA0; public function __construct($name)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;name = $name;
&#xA0; &#xA0; }
}

// test callable
function getName()
{
&#xA0; &#xA0; &#xA0; return $this-&gt;name;
}
$bob = new A(&quot;Bob&quot;);

$cl1 = Closure::fromCallable(&quot;getName&quot;);
$cl1 = $cl1-&gt;bindTo($bob, &apos;A&apos;);

//This will retrieve: Uncaught Error: Cannot access private property A::$name 
$result = $cl1();
echo $result;

//But for a Lambda function
$cl2 = function() {
&#xA0; &#xA0; return $this-&gt;name;
};
$cl2 = $cl2-&gt;bindTo($bob, &apos;A&apos;);
$result = $cl2();

// This will print Bob
echo $result;

  

#

[Official documentation page](https://www.php.net/manual/en/closure.fromcallable.php)

**[To root](/README.md)**