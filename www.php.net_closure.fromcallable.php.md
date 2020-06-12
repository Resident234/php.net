# Closure::fromCallable




<div class="phpcode"><span class="html">
It seems that the result of the &quot;fromCallable&quot; behaves a little bit different then an original Lambda function.<br><br>class A {<br>&#xA0; &#xA0; private $name;<br>&#xA0; &#xA0; public function __construct($name)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;name = $name;<br>&#xA0; &#xA0; }<br>}<br><br>// test callable<br>function getName()<br>{<br>&#xA0; &#xA0; &#xA0; return $this-&gt;name;<br>}<br>$bob = new A(&quot;Bob&quot;);<br><br>$cl1 = Closure::fromCallable(&quot;getName&quot;);<br>$cl1 = $cl1-&gt;bindTo($bob, &apos;A&apos;);<br><br>//This will retrieve: Uncaught Error: Cannot access private property A::$name <br>$result = $cl1();<br>echo $result;<br><br>//But for a Lambda function<br>$cl2 = function() {<br>&#xA0; &#xA0; return $this-&gt;name;<br>};<br>$cl2 = $cl2-&gt;bindTo($bob, &apos;A&apos;);<br>$result = $cl2();<br><br>// This will print Bob<br>echo $result;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/closure.fromcallable.php)

**[To root](/)**