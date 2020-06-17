# The Iterator interface




<div class="phpcode"><span class="html">
Order of operations when using a foreach loop:<br><br>1. Before the first iteration of the loop, Iterator::rewind() is called.<br>2. Before each iteration of the loop, Iterator::valid() is called.<br>3a. It Iterator::valid() returns false, the loop is terminated.<br>3b. If Iterator::valid() returns true, Iterator::current() and<br>Iterator::key() are called.<br>4. The loop body is evaluated.<br>5. After each iteration of the loop, Iterator::next() is called and we repeat from step 2 above.<br><br>This is roughly equivalent to:<br><br><span class="default">&lt;?php<br>$it</span><span class="keyword">-&gt;</span><span class="default">rewind</span><span class="keyword">();<br><br>while (</span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">valid</span><span class="keyword">())<br>{<br>&#xA0; &#xA0; </span><span class="default">$key </span><span class="keyword">= </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">key</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">();<br><br>&#xA0; &#xA0; </span><span class="comment">// ...<br><br>&#xA0; &#xA0; </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">();<br>}<br></span><span class="default">?&gt;<br></span><br>The loop isn&apos;t terminated until Iterator::valid() returns false or the body of the loop executes a break statement.<br><br>The only two methods that are always executed are Iterator::rewind() and Iterator::valid() (unless rewind throws an exception).<br><br>The Iterator::next() method need not return anything. It is defined as returning void. On the other hand, sometimes it is convenient for this method to return something, in which case you can do so if you want.<br><br>If your iterator is doing something expensive, like making a database query and iterating over the result set, the best place to make the query is probably in the Iterator::rewind() implementation.<br><br>In this case, the construction of the iterator itself can be cheap, and after construction you can continue to set the properties of the query all the way up to the beginning of the foreach loop since the<br>Iterator::rewind() method isn&apos;t called until then.<br><br>Things to keep in mind when making a database result set iterator:<br><br>* Make sure you close your cursor or otherwise clean up any previous query at the top of the rewind method. Otherwise your code will break if the same iterator is used in two consecutive foreach loops when the first loop terminates with a break statement before all the results are iterated over.<br><br>* Make sure your rewind() implementation tries to grab the first result so that the subsequent call to valid() will know whether or not the result set is empty. I do this by explicitly calling next() from the end of my rewind() implementation.<br><br>* For things like result set iterators, there really isn&apos;t always a &quot;key&quot; that you can return, unless you know you have a scalar primary key column in the query. Unfortunately, there will be cases where either the iterator doesn&apos;t know the primary key column because it isn&apos;t providing the query, the nature of the query is such that a primary key isn&apos;t applicable, the iterator is iterating over a table that doesn&apos;t have one, or the iterator is iterating over a table that has a compound primary key. In these cases, key() can return either:<br>the row index (based on a simple counter that you provide), or can simply return null.<br><br>Iterators can also be used to:<br><br>* iterate over the lines of a file or rows of a CSV file<br>* iterate over the characters of a string<br>* iterate over the tokens in an input stream<br>* iterate over the matches returned by an xpath expression<br>* iterate over the matches returned by a regexp<br>* iterate over the files in a folder<br>* etc...</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment"># - Here is an implementation of the Iterator interface for arrays<br>#&#xA0; &#xA0;&#xA0; which works with maps (key/value pairs)<br>#&#xA0; &#xA0;&#xA0; as well as traditional arrays<br>#&#xA0; &#xA0;&#xA0; (contiguous monotonically increasing indexes).<br>#&#xA0;&#xA0; Though it pretty much does what an array<br>#&#xA0; &#xA0;&#xA0; would normally do within foreach() loops,<br>#&#xA0; &#xA0;&#xA0; this class may be useful for using arrays<br>#&#xA0; &#xA0;&#xA0; with code that generically/only supports the<br>#&#xA0; &#xA0;&#xA0; Iterator interface.<br>#&#xA0; Another use of this class is to simply provide<br>#&#xA0; &#xA0;&#xA0; object methods with tightly controlling iteration of arrays.<br><br></span><span class="keyword">class </span><span class="default">tIterator_array </span><span class="keyword">implements </span><span class="default">Iterator </span><span class="keyword">{<br>&#xA0; private </span><span class="default">$myArray</span><span class="keyword">;<br><br>&#xA0; public function </span><span class="default">__construct</span><span class="keyword">( </span><span class="default">$givenArray </span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray </span><span class="keyword">= </span><span class="default">$givenArray</span><span class="keyword">;<br>&#xA0; }<br>&#xA0; function </span><span class="default">rewind</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">reset</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; function </span><span class="default">current</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">current</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; function </span><span class="default">key</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">key</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; function </span><span class="default">next</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">next</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; function </span><span class="default">valid</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">key</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">myArray</span><span class="keyword">) !== </span><span class="default">null</span><span class="keyword">;<br>&#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you have a custom iterator that may throw an exception in it&apos;s current() method, there is no way to catch the exception without breaking a foreach loop.<br><br>The following for loop allows you to skip elements for which $iterator-&gt;current() throws an exception, rather than breaking the loop.<br><br><span class="default">&lt;?php<br></span><span class="keyword">for (</span><span class="default">$iterator</span><span class="keyword">-&gt;</span><span class="default">rewind</span><span class="keyword">(); </span><span class="default">$iterator</span><span class="keyword">-&gt;</span><span class="default">valid</span><span class="keyword">(); </span><span class="default">$iterator</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">()) {<br>&#xA0; &#xA0; try {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">$iterator</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">();<br>&#xA0; &#xA0; } catch (</span><span class="default">Exception $exception</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; continue;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment"># ...<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s important to note that following won&apos;t work if you have null values.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">valid</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">__METHOD__</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">array</span><span class="keyword">[</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">position</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Other examples have shown the following which won&apos;t work if you have false values:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">valid</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">() !== </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Instead use:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">valid</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">array</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">position</span><span class="keyword">);<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Or the following if you do not store the position.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">valid</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return !</span><span class="default">is_null</span><span class="keyword">(</span><span class="default">key</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">array</span><span class="keyword">));<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
So, playing around with iterators in PHP (coming from languages where I&apos;m spoiled with generators to do things like this), I wrote a quick piece of code to give the Fibonacci sequence (to infinity, though only the first terms up to F_{10} are output).
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">Fibonacci </span><span class="keyword">implements </span><span class="default">Iterator </span><span class="keyword">{
<br>&#xA0; &#xA0; private </span><span class="default">$previous </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$current </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$key </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">current</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">key</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">key</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">next</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newprevious </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current </span><span class="keyword">+= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">previous</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">previous </span><span class="keyword">= </span><span class="default">$newprevious</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">key</span><span class="keyword">++;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">rewind</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">previous </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">current </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">key </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">valid</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$seq </span><span class="keyword">= new </span><span class="default">Fibonacci</span><span class="keyword">;
<br></span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>foreach (</span><span class="default">$seq </span><span class="keyword">as </span><span class="default">$f</span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$f</span><span class="string">\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; if (</span><span class="default">$i</span><span class="keyword">++ === </span><span class="default">10</span><span class="keyword">) break;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.iterator.php)

**[To root](/README.md)**