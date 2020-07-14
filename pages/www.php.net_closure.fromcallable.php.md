# Closure::fromCallable



It seems that the result of the "fromCallable" behaves a little bit different then an original Lambda function.<br><br>class A {<br>    private $name;<br>    public function __construct($name)<br>    {<br>        $this-&gt;name = $name;<br>    }<br>}<br><br>// test callable<br>function getName()<br>{<br>      return $this-&gt;name;<br>}<br>$bob = new A("Bob");<br><br>$cl1 = Closure::fromCallable("getName");<br>$cl1 = $cl1-&gt;bindTo($bob, &apos;A&apos;);<br><br>//This will retrieve: Uncaught Error: Cannot access private property A::$name <br>$result = $cl1();<br>echo $result;<br><br>//But for a Lambda function<br>$cl2 = function() {<br>    return $this-&gt;name;<br>};<br>$cl2 = $cl2-&gt;bindTo($bob, &apos;A&apos;);<br>$result = $cl2();<br><br>// This will print Bob<br>echo $result;  

#

[Official documentation page](https://www.php.net/manual/en/closure.fromcallable.php)

**[To root](/README.md)**