# Class Abstraction




<div class="phpcode"><span class="html">
Just one more time, in the simplest terms possible:<br><br>An Interface is like a protocol. It doesn&apos;t designate the behavior of the object; it designates how your code tells that object to act. An interface would be like the English Language: defining an interface defines how your code communicates with any object implementing that interface.<br><br>An interface is always an agreement or a promise. When a class says &quot;I implement interface Y&quot;, it is saying &quot;I promise to have the same public methods that any object with interface Y has&quot;.<br><br>On the other hand, an Abstract Class is like a partially built class. It is much like a document with blanks to fill in. It might be using English, but that isn&apos;t as important as the fact that some of the document is already written.<br><br>An abstract class is the foundation for another object. When a class says &quot;I extend abstract class Y&quot;, it is saying &quot;I use some methods or properties already defined in this other class named Y&quot;.<br><br>So, consider the following PHP:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">X </span><span class="keyword">implements </span><span class="default">Y </span><span class="keyword">{ } </span><span class="comment">// this is saying that &quot;X&quot; agrees to speak language &quot;Y&quot; with your code.<br><br></span><span class="keyword">class </span><span class="default">X </span><span class="keyword">extends </span><span class="default">Y </span><span class="keyword">{ } </span><span class="comment">// this is saying that &quot;X&quot; is going to complete the partial class &quot;Y&quot;.<br></span><span class="default">?&gt;<br></span><br>You would have your class implement a particular interface if you were distributing a class to be used by other people. The interface is an agreement to have a specific set of public methods for your class.<br><br>You would have your class extend an abstract class if you (or someone else) wrote a class that already had some methods written that you want to use in your new class.<br><br>These concepts, while easy to confuse, are specifically different and distinct. For all intents and purposes, if you&apos;re the only user of any of your classes, you don&apos;t need to implement interfaces.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s an example that helped me with understanding abstract classes. It&apos;s just a very simple way of explaining it (in my opinion). Lets say we have the following code:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">Fruit </span><span class="keyword">{
<br>&#xA0; &#xA0; private </span><span class="default">$color</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">eat</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//chew
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">setColor</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">color </span><span class="keyword">= </span><span class="default">$c</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>class </span><span class="default">Apple </span><span class="keyword">extends </span><span class="default">Fruit </span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">eat</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//chew until core
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>}
<br>
<br>class </span><span class="default">Orange </span><span class="keyword">extends </span><span class="default">Fruit </span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">eat</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//peel
<br>&#xA0; &#xA0; &#xA0; &#xA0; //chew
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Now I give you an apple and you eat it.
<br>
<br><span class="default">&lt;?php
<br>$apple </span><span class="keyword">= new </span><span class="default">Apple</span><span class="keyword">();
<br></span><span class="default">$apple</span><span class="keyword">-&gt;</span><span class="default">eat</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>What does it taste like? It tastes like an apple. Now I give you a fruit.
<br>
<br><span class="default">&lt;?php
<br>$fruit </span><span class="keyword">= new </span><span class="default">Fruit</span><span class="keyword">();
<br></span><span class="default">$fruit</span><span class="keyword">-&gt;</span><span class="default">eat</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>What does that taste like??? Well, it doesn&apos;t make much sense, so you shouldn&apos;t be able to do that. This is accomplished by making the Fruit class abstract as well as the eat method inside of it.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">abstract class </span><span class="default">Fruit </span><span class="keyword">{
<br>&#xA0; &#xA0; private </span><span class="default">$color</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; abstract public function </span><span class="default">eat</span><span class="keyword">();
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">setColor</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">color </span><span class="keyword">= </span><span class="default">$c</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Now just think about a Database class where MySQL and PostgreSQL extend it. Also, a note. An abstract class is just like an interface, but you can define methods in an abstract class whereas in an interface they are all abstract.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Abstraction and interfaces are two very different tools. The are as close as hammers and drills. Abstract classes may have implemented methods, whereas interfaces have no implementation in themselves.<br><br>Abstract classes that declare all their methods as abstract are not interfaces with different names. One can implement multiple interfaces, but not extend multiple classes (or abstract classes).<br><br>The use of abstraction vs interfaces is problem specific and the choice is made during the design of software, not its implementation. In the same project you may as well offer an interface and a base (probably abstract) class as a reference that implements the interface. Why would you do that?<br><br>Let us assume that we want to build a system that calls different services, which in turn have actions. Normally, we could offer a method called execute that accepts the name of the action as a parameter and executes the action.<br><br>We want to make sure that classes can actually define their own ways of executing actions. So we create an interface IService that has the execute method. Well, in most of your cases, you will be copying and pasting the exact same code for execute.<br><br>We can create a reference implemention for a class named Service and implement the execute method. So, no more copying and pasting for your other classes! But what if you want to extend MySLLi?? You can implement the interface (copy-paste probably), and there you are, again with a service. Abstraction can be included in the class for initialisation code, which cannot be predefined for every class that you will write.<br><br>Hope this is not too mind-boggling and helps someone. Cheers,<br>Alexios Tsiaparas</span>
</div>
  

#


<div class="phpcode"><span class="html">
The documentation says: &quot;It is not allowed to create an instance of a class that has been defined as abstract.&quot;. It only means you cannot initialize an object from an abstract class. Invoking static method of abstract class is still feasible. For example:<br><span class="default">&lt;?php<br></span><span class="keyword">abstract class </span><span class="default">Foo<br></span><span class="keyword">{<br>&#xA0; &#xA0; static function </span><span class="default">bar</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;test\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">Foo</span><span class="keyword">::</span><span class="default">bar</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This example will hopefully help you see how abstract works, how interfaces work, and how they can work together. This example will also work/compile on PHP7, the others were typed live in the form and may work but the last one was made/tested for real:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">const </span><span class="default">&#xB6; </span><span class="keyword">= </span><span class="default">PHP_EOL</span><span class="keyword">;<br><br></span><span class="comment">// Define things a product *has* to be able to do (has to implement)<br></span><span class="keyword">interface </span><span class="default">productInterface </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doSell</span><span class="keyword">();<br>&#xA0; &#xA0; public function </span><span class="default">doBuy</span><span class="keyword">();<br>}<br><br></span><span class="comment">// Define our default abstraction<br></span><span class="keyword">abstract class </span><span class="default">defaultProductAbstraction </span><span class="keyword">implements </span><span class="default">productInterface </span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$_bought </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; private </span><span class="default">$_sold </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; abstract public function </span><span class="default">doMore</span><span class="keyword">();<br>&#xA0; &#xA0; public function </span><span class="default">doSell</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* the default implementation */<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_sold </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;defaultProductAbstraction doSell: </span><span class="keyword">{</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_sold</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">doBuy</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_bought </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;defaultProductAbstraction doBuy: </span><span class="keyword">{</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_bought</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">defaultProductImplementation </span><span class="keyword">extends </span><span class="default">defaultProductAbstraction </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doMore</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;defaultProductImplementation doMore()&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">myProductImplementation </span><span class="keyword">extends </span><span class="default">defaultProductAbstraction </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doMore</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;myProductImplementation doMore() does more!&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">doBuy</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;myProductImplementation&apos;s doBuy() and also my parent&apos;s dubai()&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">doBuy</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">myProduct </span><span class="keyword">extends </span><span class="default">defaultProductImplementation </span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$_bought</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_bought</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">doBuy </span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* non-default doBuy implementation */<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_bought </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;myProduct overrides the defaultProductImplementation&apos;s doBuy() here </span><span class="keyword">{</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_bought</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">myOtherProduct </span><span class="keyword">extends </span><span class="default">myProductImplementation </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doBuy</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;myOtherProduct overrides myProductImplementations doBuy() here but still calls parent too&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">doBuy</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>}<br><br>echo </span><span class="string">&quot;new myProduct()&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br></span><span class="default">$product </span><span class="keyword">= new </span><span class="default">myProduct</span><span class="keyword">();<br><br></span><span class="default">$product</span><span class="keyword">-&gt;</span><span class="default">doBuy</span><span class="keyword">();<br></span><span class="default">$product</span><span class="keyword">-&gt;</span><span class="default">doSell</span><span class="keyword">();<br></span><span class="default">$product</span><span class="keyword">-&gt;</span><span class="default">doMore</span><span class="keyword">();<br><br>echo </span><span class="default">&#xB6;</span><span class="keyword">.</span><span class="string">&quot;new defaultProductImplementation()&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br><br></span><span class="default">$newProduct </span><span class="keyword">= new </span><span class="default">defaultProductImplementation</span><span class="keyword">();<br></span><span class="default">$newProduct</span><span class="keyword">-&gt;</span><span class="default">doBuy</span><span class="keyword">();<br></span><span class="default">$newProduct</span><span class="keyword">-&gt;</span><span class="default">doSell</span><span class="keyword">();<br></span><span class="default">$newProduct</span><span class="keyword">-&gt;</span><span class="default">doMore</span><span class="keyword">();<br><br>echo </span><span class="default">&#xB6;</span><span class="keyword">.</span><span class="string">&quot;new myProductImplementation&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br></span><span class="default">$lastProduct </span><span class="keyword">= new </span><span class="default">myProductImplementation</span><span class="keyword">();<br></span><span class="default">$lastProduct</span><span class="keyword">-&gt;</span><span class="default">doBuy</span><span class="keyword">();<br></span><span class="default">$lastProduct</span><span class="keyword">-&gt;</span><span class="default">doSell</span><span class="keyword">();<br></span><span class="default">$lastProduct</span><span class="keyword">-&gt;</span><span class="default">doMore</span><span class="keyword">();<br><br>echo </span><span class="default">&#xB6;</span><span class="keyword">.</span><span class="string">&quot;new myOtherProduct&quot;</span><span class="keyword">.</span><span class="default">&#xB6;</span><span class="keyword">;<br></span><span class="default">$anotherNewProduct </span><span class="keyword">= new </span><span class="default">myOtherProduct</span><span class="keyword">();<br></span><span class="default">$anotherNewProduct</span><span class="keyword">-&gt;</span><span class="default">doBuy</span><span class="keyword">();<br></span><span class="default">$anotherNewProduct</span><span class="keyword">-&gt;</span><span class="default">doSell</span><span class="keyword">();<br></span><span class="default">$anotherNewProduct</span><span class="keyword">-&gt;</span><span class="default">doMore</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>Will result in:<br><span class="default">&lt;?php<br></span><span class="comment">/*<br>new myProduct()<br>bool(true)<br>myProduct overrides the defaultProductImplementation&apos;s doBuy() here 1<br>defaultProductAbstraction doSell: 1<br>defaultProductImplementation doMore()<br><br>new defaultProductImplementation()<br>defaultProductAbstraction doBuy: 1<br>defaultProductAbstraction doSell: 1<br>defaultProductImplementation doMore()<br><br>new myProductImplementation<br>myProductImplementation&apos;s doBuy() and also my parent&apos;s dubai()<br>defaultProductAbstraction doBuy: 1<br>defaultProductAbstraction doSell: 1<br>myProductImplementation doMore() does more!<br><br>new myOtherProduct<br>myOtherProduct overrides myProductImplementations doBuy() here but still calls parent too<br>myProductImplementation&apos;s doBuy() and also my parent&apos;s dubai()<br>defaultProductAbstraction doBuy: 1<br>defaultProductAbstraction doSell: 1<br>myProductImplementation doMore() does more!<br><br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Ok...the docs are a bit vague when it comes to an abstract class extending another abstract class.&#xA0; An abstract class that extends another abstract class doesn&apos;t need to define the abstract methods from the parent class.&#xA0; In other words, this causes an error:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">abstract class </span><span class="default">class1 </span><span class="keyword">{
<br>&#xA0; abstract public function </span><span class="default">someFunc</span><span class="keyword">();
<br>}
<br>abstract class </span><span class="default">class2 </span><span class="keyword">extends </span><span class="default">class1 </span><span class="keyword">{
<br>&#xA0; abstract public function </span><span class="default">someFunc</span><span class="keyword">();
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Error: Fatal error: Can&apos;t inherit abstract function class1::someFunc() (previously declared abstract in class2) in /home/sneakyimp/public/chump.php on line 7
<br>
<br>However this does not:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">abstract class </span><span class="default">class1 </span><span class="keyword">{
<br>&#xA0; abstract public function </span><span class="default">someFunc</span><span class="keyword">();
<br>}
<br>abstract class </span><span class="default">class2 </span><span class="keyword">extends </span><span class="default">class1 </span><span class="keyword">{
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>An abstract class that extends an abstract class can pass the buck to its child classes when it comes to implementing the abstract methods of its parent abstract class.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Incidentally, abstract classes do not need to be base classes:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">sneeze</span><span class="keyword">() { echo </span><span class="string">&apos;achoooo&apos;</span><span class="keyword">; }<br>}<br><br>abstract class </span><span class="default">Bar </span><span class="keyword">extends </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0; &#xA0; public abstract function </span><span class="default">hiccup</span><span class="keyword">();<br>}<br><br>class </span><span class="default">Baz </span><span class="keyword">extends </span><span class="default">Bar </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">hiccup</span><span class="keyword">() { echo </span><span class="string">&apos;hiccup!&apos;</span><span class="keyword">; }<br>}<br><br></span><span class="default">$baz </span><span class="keyword">= new </span><span class="default">Baz</span><span class="keyword">();<br></span><span class="default">$baz</span><span class="keyword">-&gt;</span><span class="default">sneeze</span><span class="keyword">();<br></span><span class="default">$baz</span><span class="keyword">-&gt;</span><span class="default">hiccup</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.abstract.php)

**[To root](/README.md)**