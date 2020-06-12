# Callbacks / Callables




<div class="phpcode"><span class="html">
You can also use the $this variable to specify a callback:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{
<br>
<br>&#xA0; &#xA0; public </span><span class="default">$property </span><span class="keyword">= </span><span class="string">&apos;Hello World!&apos;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">MyMethod</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;myCallbackMethod&apos;</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">MyCallbackMethod</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">property</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Performance note: The callable type hint, like is_callable(), will trigger an autoload of the class if the value looks like a static method callback.</span>
</div>
  

#


<div class="phpcode"><span class="html">
A note on differences when calling callbacks as &quot;variable functions&quot; without the use of call_user_func() (e.g. &quot;<span class="default">&lt;?php $callback </span><span class="keyword">= </span><span class="string">&apos;printf&apos;</span><span class="keyword">; </span><span class="default">$callback</span><span class="keyword">(</span><span class="string">&apos;Hello World!&apos;</span><span class="keyword">) </span><span class="default">?&gt;</span>&quot;):<br><br>- Using the name of a function as string has worked since at least 4.3.0<br>- Calling anonymous functions and invokable objects has worked since 5.3.0<br>- Using the array structure [$object, &apos;method&apos;] has worked since 5.4.0<br><br>Note, however, that the following are not supported when calling callbacks as variable functions, even though they are supported by call_user_func():<br><br>- Calling static class methods via strings such as &apos;foo::doStuff&apos;<br>- Calling parent method using the [$object, &apos;parent::method&apos;] array structure<br><br>All of these cases are correctly recognized as callbacks by the &apos;callable&apos; type hint, however. Thus, the following code will produce an error &quot;Fatal error: Call to undefined function foo::doStuff() in /tmp/code.php on line 4&quot;:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">foo </span><span class="keyword">{<br>&#xA0; &#xA0; static function </span><span class="default">callIt</span><span class="keyword">(callable </span><span class="default">$callback</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$callback</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; static function </span><span class="default">doStuff</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Hello World!&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">foo</span><span class="keyword">::</span><span class="default">callIt</span><span class="keyword">(</span><span class="string">&apos;foo::doStuff&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The code would work fine, if we replaced the &apos;$callback()&apos; with &apos;call_user_func($callback)&apos; or if we used the array [&apos;foo&apos;, &apos;doStuff&apos;] as the callback instead.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use &apos;self::methodName&apos; as a callable, but this is dangerous. Consider this example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0; &#xA0; public static function </span><span class="default">doAwesomeThings</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">FunctionCaller</span><span class="keyword">::</span><span class="default">callIt</span><span class="keyword">(</span><span class="string">&apos;self::someAwesomeMethod&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public static function </span><span class="default">someAwesomeMethod</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// fantastic code goes here.<br>&#xA0; &#xA0; </span><span class="keyword">}<br>}<br><br>class </span><span class="default">FunctionCaller </span><span class="keyword">{<br>&#xA0; &#xA0; public static function </span><span class="default">callIt</span><span class="keyword">(callable </span><span class="default">$func</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$func</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">Foo</span><span class="keyword">::</span><span class="default">doAwesomeThings</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>This results in an error:<br>Warning: class &apos;FunctionCaller&apos; does not have a method &apos;someAwesomeMethod&apos;.<br><br>For this reason you should always use the full class name:<br><span class="default">&lt;?php<br>FunctionCaller</span><span class="keyword">::</span><span class="default">callIt</span><span class="keyword">(</span><span class="string">&apos;Foo::someAwesomeMethod&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>I believe this is because there is no way for FunctionCaller to know that the string &apos;self&apos; at one point referred to to `Foo`.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When specifying a call back in array notation (ie. array($this, &quot;myfunc&quot;) ) the method can be private if called from inside the class, but if you call it from outside you&apos;ll get a warning:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">mc </span><span class="keyword">{
<br>&#xA0;&#xA0; public function </span><span class="default">go</span><span class="keyword">(array </span><span class="default">$arr</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">array_walk</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&quot;walkIt&quot;</span><span class="keyword">));
<br>&#xA0;&#xA0; }
<br>
<br>&#xA0;&#xA0; private function </span><span class="default">walkIt</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0;&#xA0; echo </span><span class="default">$val </span><span class="keyword">. </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;
<br>&#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">export</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;walkIt&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$data </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">);
<br>
<br></span><span class="default">$m </span><span class="keyword">= new </span><span class="default">mc</span><span class="keyword">;
<br></span><span class="default">$m</span><span class="keyword">-&gt;</span><span class="default">go</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">); </span><span class="comment">// valid
<br>
<br></span><span class="default">array_walk</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">, </span><span class="default">$m</span><span class="keyword">-&gt;</span><span class="default">export</span><span class="keyword">()); </span><span class="comment">// will generate warning
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Output:
<br>1&lt;br /&gt;2&lt;br /&gt;3&lt;br /&gt;4&lt;br /&gt;
<br>Warning: array_walk() expects parameter 2 to be a valid callback, cannot access private method mc::walkIt() in /in/tfh7f on line 22</span>
</div>
  

#


<div class="phpcode"><span class="html">
you can pass an object as a callable if its class defines the __invoke() magic method..</span>
</div>
  

#


<div class="phpcode"><span class="html">
&gt; As of PHP 5.2.3, it is also possible to pass &apos;ClassName::methodName&apos;<br><br>You can also use &apos;self::methodName&apos;.&#xA0; This works in PHP 5.2.12 for me.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I needed a function that would determine the type of callable being passed, and, eventually,<br>normalized it to some extent. Here&apos;s what I came up with:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * The callable types and normalizations are given in the table below:<br> *<br> *&#xA0; Callable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | Normalization&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | Type<br> * ---------------------------------+---------------------------------+--------------<br> *&#xA0; function (...) use (...) {...}&#xA0; | function (...) use (...) {...}&#xA0; | &apos;closure&apos;<br> *&#xA0; $object&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | $object&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;invocable&apos;<br> *&#xA0; &quot;function&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | &quot;function&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | &apos;function&apos;<br> *&#xA0; &quot;class::method&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;static&apos;<br> *&#xA0; [&quot;class&quot;, &quot;parent::method&quot;]&#xA0; &#xA0;&#xA0; | [&quot;parent of class&quot;, &quot;method&quot;]&#xA0;&#xA0; | &apos;static&apos;<br> *&#xA0; [&quot;class&quot;, &quot;self::method&quot;]&#xA0; &#xA0; &#xA0;&#xA0; | [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;static&apos;<br> *&#xA0; [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;static&apos;<br> *&#xA0; [$object, &quot;parent::method&quot;]&#xA0; &#xA0;&#xA0; | [$object, &quot;parent::method&quot;]&#xA0; &#xA0;&#xA0; | &apos;object&apos;<br> *&#xA0; [$object, &quot;self::method&quot;]&#xA0; &#xA0; &#xA0;&#xA0; | [$object, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;object&apos;<br> *&#xA0; [$object, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | [$object, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;object&apos;<br> * ---------------------------------+---------------------------------+--------------<br> *&#xA0; other callable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | idem&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | &apos;unknown&apos;<br> * ---------------------------------+---------------------------------+--------------<br> *&#xA0; not a callable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | null&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | false<br> *<br> * If the &quot;strict&quot; parameter is set to true, additional checks are<br> * performed, in particular:<br> *&#xA0; - when a callable string of the form &quot;class::method&quot; or a callable array<br> *&#xA0; &#xA0; of the form [&quot;class&quot;, &quot;method&quot;] is given, the method must be a static one,<br> *&#xA0; - when a callable array of the form [$object, &quot;method&quot;] is given, the<br> *&#xA0; &#xA0; method must be a non-static one.<br> *<br> */<br></span><span class="keyword">function </span><span class="default">callableType</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">, </span><span class="default">$strict </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">, callable&amp; </span><span class="default">$norm </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; if (!</span><span class="default">is_callable</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">)) {<br>&#xA0; &#xA0; switch (</span><span class="default">true</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; case </span><span class="default">is_object</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">):<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$norm </span><span class="keyword">= </span><span class="default">$callable</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;Closure&apos; </span><span class="keyword">=== </span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">) ? </span><span class="string">&apos;closure&apos; </span><span class="keyword">: </span><span class="string">&apos;invocable&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; case </span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">):<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$m&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;~^(?&lt;class&gt;[a-z_][a-z0-9_]*)::(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i&apos;</span><span class="keyword">, </span><span class="default">$callable</span><span class="keyword">, </span><span class="default">$m</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">) = [</span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;class&apos;</span><span class="keyword">], </span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;method&apos;</span><span class="keyword">]];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$strict </span><span class="keyword">|| (new \</span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">))-&gt;</span><span class="default">isStatic</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$norm </span><span class="keyword">= [</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;static&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$norm </span><span class="keyword">= </span><span class="default">$callable</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;function&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; case </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">):<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$m </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;~^(:?(?&lt;reference&gt;self|parent)::)?(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i&apos;</span><span class="keyword">, </span><span class="default">$callable</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$m</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="string">&apos;parent&apos; </span><span class="keyword">=== </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;reference&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">) = [</span><span class="default">get_parent_class</span><span class="keyword">(</span><span class="default">$callable</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]), </span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;method&apos;</span><span class="keyword">]];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">) = [</span><span class="default">$callable</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;method&apos;</span><span class="keyword">]];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$strict </span><span class="keyword">|| (new \</span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">))-&gt;</span><span class="default">isStatic</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$norm </span><span class="keyword">= [</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;static&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="string">&apos;self&apos; </span><span class="keyword">=== </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;reference&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">) = [</span><span class="default">$callable</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$m</span><span class="keyword">[</span><span class="string">&apos;method&apos;</span><span class="keyword">]];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">) = </span><span class="default">$callable</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$strict </span><span class="keyword">|| !(new \</span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">))-&gt;</span><span class="default">isStatic</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$norm </span><span class="keyword">= [</span><span class="default">$left</span><span class="keyword">, </span><span class="default">$right</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;object&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">$norm </span><span class="keyword">= </span><span class="default">$callable</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="string">&apos;unknown&apos;</span><span class="keyword">;<br>&#xA0; }<br>&#xA0; </span><span class="default">$norm </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; return </span><span class="default">false</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>Hope someone else finds it useful.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When trying to make a callable from a function name located in a namespace, you MUST give the fully qualified function name (regardless of the current namespace or use statements).<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">namespace </span><span class="default">MyNamespace</span><span class="keyword">;<br><br>function </span><span class="default">doSomethingFancy</span><span class="keyword">(</span><span class="default">$arg1</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// do something...<br></span><span class="keyword">}<br><br></span><span class="default">$values </span><span class="keyword">= [</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">];<br><br></span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;doSomethingFancy&apos;</span><span class="keyword">, </span><span class="default">$values</span><span class="keyword">);<br></span><span class="comment">// array_map() expects parameter 1 to be a valid callback, function &apos;doSomethingFancy&apos; not found or invalid function name<br><br></span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;MyNamespace\doSomethingFancy&apos;</span><span class="keyword">, </span><span class="default">$values</span><span class="keyword">);<br></span><span class="comment">// =&gt; [..., ..., ...]</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.callable.php)

**[⬆ to root](/)**