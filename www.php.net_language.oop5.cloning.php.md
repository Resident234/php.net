# Object Cloning




<div class="phpcode"><span class="html">
I think it&apos;s relevant to note that __clone is NOT an override. As the example shows, the normal cloning process always occurs, and it&apos;s the responsibility of the __clone method to &quot;mend&quot; any &quot;wrong&quot; action performed by it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is test script i wrote to test the behaviour of clone when i have arrays with primitive values in my class - as an additonal test of the note below by jeffrey at whinger dot nl<br><br>&lt;pre&gt;<br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{<br><br>&#xA0; &#xA0; private </span><span class="default">$myArray </span><span class="keyword">= array();<br>&#xA0; &#xA0; function </span><span class="default">pushSomethingToArray</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; function </span><span class="default">getArray</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="comment">//push some values to the myArray of Mainclass<br></span><span class="default">$myObj </span><span class="keyword">= new </span><span class="default">MyClass</span><span class="keyword">();<br></span><span class="default">$myObj</span><span class="keyword">-&gt;</span><span class="default">pushSomethingToArray</span><span class="keyword">(</span><span class="string">&apos;blue&apos;</span><span class="keyword">);<br></span><span class="default">$myObj</span><span class="keyword">-&gt;</span><span class="default">pushSomethingToArray</span><span class="keyword">(</span><span class="string">&apos;orange&apos;</span><span class="keyword">);<br></span><span class="default">$myObjClone </span><span class="keyword">= clone </span><span class="default">$myObj</span><span class="keyword">;<br></span><span class="default">$myObj</span><span class="keyword">-&gt;</span><span class="default">pushSomethingToArray</span><span class="keyword">(</span><span class="string">&apos;pink&apos;</span><span class="keyword">);<br><br></span><span class="comment">//testing<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$myObj</span><span class="keyword">-&gt;</span><span class="default">getArray</span><span class="keyword">());&#xA0; &#xA0;&#xA0; </span><span class="comment">//Array([0] =&gt; blue,[1] =&gt; orange,[2] =&gt; pink)<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$myObjClone</span><span class="keyword">-&gt;</span><span class="default">getArray</span><span class="keyword">());</span><span class="comment">//Array([0] =&gt; blue,[1] =&gt; orange)<br>//so array&#xA0; cloned <br><br></span><span class="default">?&gt;<br></span>&lt;/pre&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
I ran into the same problem of an array of objects inside of an object that I wanted to clone all pointing to the same objects. However, I agreed that serializing the data was not the answer. It was relatively simple, really:
<br>
<br>public function __clone() {
<br>&#xA0; &#xA0; foreach ($this-&gt;varName as &amp;$a) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach ($a as &amp;$b) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $b = clone $b;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>Note, that I was working with a multi-dimensional array and I was not using the Key=&gt;Value pair system, but basically, the point is that if you use foreach, you need to specify that the copied data is to be accessed by reference.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.cloning.php)

**[To root](/README.md)**