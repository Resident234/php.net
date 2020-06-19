# Final Keyword




<div class="phpcode"><span class="html">
Note for Java developers: the &apos;final&apos; keyword is not used for class constants in PHP. We use the keyword &apos;const&apos;.<br><br><a href="http://php.net/manual/en/language.oop5.constants.php" rel="nofollow" target="_blank">http://php.net/manual/en/language.oop5.constants.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that you cannot ovverride final methods even if they are defined as private in parent class.
<br>Thus, the following example:
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">parentClass </span><span class="keyword">{
<br>&#xA0; &#xA0; final private function </span><span class="default">someMethod</span><span class="keyword">() { }
<br>}
<br>class </span><span class="default">childClass </span><span class="keyword">extends </span><span class="default">parentClass </span><span class="keyword">{
<br>&#xA0; &#xA0; private function </span><span class="default">someMethod</span><span class="keyword">() { }
<br>}
<br></span><span class="default">?&gt;
<br></span>dies with error &quot;Fatal error: Cannot override final method parentClass::someMethod() in ***.php on line 7&quot;
<br>
<br>Such behaviour looks slight unexpected because in child class we cannot know, which private methods exists in a parent class and vice versa.
<br>
<br>So, remember that if you defined a private final method, you cannot place method with the same name in child class.</span>
</div>
  

#


<div class="phpcode"><span class="html">
@thomas at somewhere dot com<br><br>The &apos;final&apos; keyword is extremely useful.&#xA0; Inheritance is also useful, but can be abused and becomes problematic in large applications.&#xA0; If you ever come across a finalized class or method that you wish to extend, write a decorator instead.<br><br><span class="default">&lt;?php<br></span><span class="keyword">final class </span><span class="default">Foo<br></span><span class="keyword">{<br>&#xA0; &#xA0; public </span><span class="default">method doFoo</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// do something useful and return a result<br>&#xA0; &#xA0; </span><span class="keyword">}<br>}<br><br>final class </span><span class="default">FooDecorator<br></span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$foo</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">Foo $foo</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">foo </span><span class="keyword">= </span><span class="default">$foo</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">doFoo</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">foo</span><span class="keyword">-&gt;</span><span class="default">doFoo</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// ... customize result ...<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$result</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.final.php)

**[To root](/README.md)**