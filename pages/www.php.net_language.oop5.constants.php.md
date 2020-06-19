# Class Constants




<div class="phpcode"><span class="html">
it&apos;s possible to declare constant in base class, and override it in child, and access to correct value of the const from the static method is possible by &apos;get_called_class&apos; method:<br><span class="default">&lt;?php<br></span><span class="keyword">abstract class </span><span class="default">dbObject<br></span><span class="keyword">{&#xA0; &#xA0; <br>&#xA0; &#xA0; const </span><span class="default">TABLE_NAME</span><span class="keyword">=</span><span class="string">&apos;undefined&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public static function </span><span class="default">GetAll</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">get_called_class</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&quot;SELECT * FROM `&quot;</span><span class="keyword">.</span><span class="default">$c</span><span class="keyword">::</span><span class="default">TABLE_NAME</span><span class="keyword">.</span><span class="string">&quot;`&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }&#xA0; &#xA0; <br>}<br><br>class </span><span class="default">dbPerson </span><span class="keyword">extends </span><span class="default">dbObject<br></span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">TABLE_NAME</span><span class="keyword">=</span><span class="string">&apos;persons&apos;</span><span class="keyword">;<br>}<br><br>class </span><span class="default">dbAdmin </span><span class="keyword">extends </span><span class="default">dbPerson<br></span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">TABLE_NAME</span><span class="keyword">=</span><span class="string">&apos;admins&apos;</span><span class="keyword">;<br>}<br><br>echo </span><span class="default">dbPerson</span><span class="keyword">::</span><span class="default">GetAll</span><span class="keyword">().</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;</span><span class="comment">//output: &quot;SELECT * FROM `persons`&quot;<br></span><span class="keyword">echo </span><span class="default">dbAdmin</span><span class="keyword">::</span><span class="default">GetAll</span><span class="keyword">().</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;</span><span class="comment">//output: &quot;SELECT * FROM `admins`&quot;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As of PHP 5.6 you can finally define constant using math expressions, like this one:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">MyTimer </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">SEC_PER_DAY </span><span class="keyword">= </span><span class="default">60 </span><span class="keyword">* </span><span class="default">60 </span><span class="keyword">* </span><span class="default">24</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>Me happy :)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Most people miss the point in declaring constants and confuse then things by trying to declare things like functions or arrays as constants. What happens next is to try things that are more complicated then necessary and sometimes lead to bad coding practices. Let me explain...
<br>
<br>A constant is a name for a value (but it&apos;s NOT a variable), that usually will be replaced in the code while it gets COMPILED and NOT at runtime. 
<br>
<br>So returned values from functions can&apos;t be used, because they will return a value only at runtime. 
<br>
<br>Arrays can&apos;t be used, because they are data structures that exist at runtime. 
<br>
<br>One main purpose of declaring a constant is usually using a value in your code, that you can replace easily in one place without looking for all the occurences. Another is, to avoid mistakes. 
<br>
<br>Think about some examples written by some before me: 
<br>
<br>1. const MY_ARR = &quot;return array(\&quot;A\&quot;, \&quot;B\&quot;, \&quot;C\&quot;, \&quot;D\&quot;);&quot;;
<br>It was said, this would declare an array that can be used with eval. WRONG! This is just a string as constant, NOT an array. Does it make sense if it would be possible to declare an array as constant? Probably not. Instead declare the values of the array as constants and make an array variable. 
<br>
<br>2. const magic_quotes = (bool)get_magic_quotes_gpc();
<br>This can&apos;t work, of course. And it doesn&apos;t make sense either. The function already returns the value, there is no purpose in declaring a constant for the same thing. 
<br>
<br>3. Someone spoke about &quot;dynamic&quot; assignments to constants. What? There are no dynamic assignments to constants, runtime assignments work _only_ with variables. Let&apos;s take the proposed example: 
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * Constants that deal only with the database
<br> */
<br></span><span class="keyword">class </span><span class="default">DbConstant </span><span class="keyword">extends </span><span class="default">aClassConstant </span><span class="keyword">{
<br>&#xA0; &#xA0; protected </span><span class="default">$host </span><span class="keyword">= </span><span class="string">&apos;localhost&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; protected </span><span class="default">$user </span><span class="keyword">= </span><span class="string">&apos;user&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; protected </span><span class="default">$password </span><span class="keyword">= </span><span class="string">&apos;pass&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; protected </span><span class="default">$database </span><span class="keyword">= </span><span class="string">&apos;db&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; protected </span><span class="default">$time</span><span class="keyword">;
<br>&#xA0; &#xA0; function </span><span class="default">__construct</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">time </span><span class="keyword">= </span><span class="default">time</span><span class="keyword">() + </span><span class="default">1</span><span class="keyword">; </span><span class="comment">// dynamic assignment
<br>&#xA0; &#xA0; </span><span class="keyword">}
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Those aren&apos;t constants, those are properties of the class. Something like &quot;this-&gt;time = time()&quot; would even totally defy the purpose of a constant. Constants are supposed to be just that, constant values, on every execution. They are not supposed to change every time a script runs or a class is instantiated. 
<br>
<br>Conclusion: Don&apos;t try to reinvent constants as variables. If constants don&apos;t work, just use variables. Then you don&apos;t need to reinvent methods to achieve things for what is already there.</span>
</div>
  

#


<div class="phpcode"><span class="html">
const can also be used directly in namespaces, a feature never explicitly stated in the documentation.<br><br><span class="default">&lt;?php<br></span><span class="comment"># foo.php<br></span><span class="keyword">namespace </span><span class="default">Foo</span><span class="keyword">;<br><br>const </span><span class="default">BAR </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="comment"># bar.php<br></span><span class="keyword">require </span><span class="string">&apos;foo.php&apos;</span><span class="keyword">;<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">Foo</span><span class="keyword">\</span><span class="default">BAR</span><span class="keyword">); </span><span class="comment">// =&gt; int(1)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I think it&apos;s useful if we draw some attention to late static binding here:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">MY_CONST </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; public function </span><span class="default">my_const_self</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">self</span><span class="keyword">::</span><span class="default">MY_CONST</span><span class="keyword">;<br>&#xA0; &#xA0; } <br>&#xA0; &#xA0; public function </span><span class="default">my_const_static</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return static::</span><span class="default">MY_CONST</span><span class="keyword">;<br>&#xA0; &#xA0; } <br>}<br><br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A </span><span class="keyword">{<br>&#xA0;&#xA0; const </span><span class="default">MY_CONST </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>}<br><br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">B</span><span class="keyword">();<br>echo </span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">my_const_self </span><span class="keyword">? </span><span class="string">&apos;yes&apos; </span><span class="keyword">: </span><span class="string">&apos;no&apos;</span><span class="keyword">; </span><span class="comment">// output: no<br></span><span class="keyword">echo </span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">my_const_static </span><span class="keyword">? </span><span class="string">&apos;yes&apos; </span><span class="keyword">: </span><span class="string">&apos;no&apos;</span><span class="keyword">; </span><span class="comment">// output: yes<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi, i would like to point out difference between self::CONST and $this::CONST with extended class.<br>Let us have class a:<br><br><span class="default">&lt;?php <br></span><span class="keyword">class </span><span class="default">a </span><span class="keyword">{&#xA0; &#xA0; <br>&#xA0; &#xA0; const </span><span class="default">CONST_INT </span><span class="keyword">= </span><span class="default">10</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">getSelf</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">self</span><span class="keyword">::</span><span class="default">CONST_INT</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">getThis</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">::</span><span class="default">CONST_INT</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>And class b (which extends a)<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">b </span><span class="keyword">extends </span><span class="default">a </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">CONST_INT </span><span class="keyword">= </span><span class="default">20</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">getSelf</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">getSelf</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">getThis</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">getThis</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>Both classes have same named constant CONST_INT.<br>When child call method in parent class, there is different output between self and $this usage.<br><br><span class="default">&lt;?php<br>$b </span><span class="keyword">= new </span><span class="default">b</span><span class="keyword">();<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">getSelf</span><span class="keyword">());&#xA0; &#xA0;&#xA0; </span><span class="comment">//10<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">getThis</span><span class="keyword">());&#xA0; &#xA0;&#xA0; </span><span class="comment">//20<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
[Editor&apos;s note: that is already possible as of PHP 5.6.0.]
<br>
<br>Note, as of PHP7 it is possible to define class constants with an array.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">MyClass
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; const </span><span class="default">ABC </span><span class="keyword">= array(</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;B&apos;</span><span class="keyword">, </span><span class="string">&apos;C&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; const </span><span class="default">A </span><span class="keyword">= </span><span class="string">&apos;1&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; const </span><span class="default">B </span><span class="keyword">= </span><span class="string">&apos;2&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; const </span><span class="default">C </span><span class="keyword">= </span><span class="string">&apos;3&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; const </span><span class="default">NUMBERS </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">A</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">B</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">C</span><span class="keyword">,
<br>&#xA0; &#xA0; );
<br>}
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">MyClass</span><span class="keyword">::</span><span class="default">ABC</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">MyClass</span><span class="keyword">::</span><span class="default">NUMBERS</span><span class="keyword">);
<br>
<br></span><span class="comment">// Result:
<br>/*
<br>array(3) {
<br>&#xA0; &#xA0; [0]=&gt;
<br>&#xA0; string(1) &quot;A&quot;
<br>&#xA0; &#xA0; [1]=&gt;
<br>&#xA0; string(1) &quot;B&quot;
<br>&#xA0; &#xA0; [2]=&gt;
<br>&#xA0; string(1) &quot;C&quot;
<br>}
<br>array(3) {
<br>&#xA0; &#xA0; [0]=&gt;
<br>&#xA0; string(1) &quot;1&quot;
<br>&#xA0; &#xA0; [1]=&gt;
<br>&#xA0; string(1) &quot;2&quot;
<br>&#xA0; &#xA0; [2]=&gt;
<br>&#xA0; string(1) &quot;3&quot;
<br>}
<br>*/
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Use CONST to set UPPER and LOWER LIMITS<br><br>If you have code that accepts user input or you just need to make sure input is acceptable, you can use constants to set upper and lower limits. Note: a static function that enforces your limits is highly recommended... sniff the clamp() function below for a taste.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Dimension<br></span><span class="keyword">{<br>&#xA0; const </span><span class="default">MIN </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">, </span><span class="default">MAX </span><span class="keyword">= </span><span class="default">800</span><span class="keyword">;<br><br>&#xA0; public </span><span class="default">$width</span><span class="keyword">, </span><span class="default">$height</span><span class="keyword">;<br><br>&#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$w </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">, </span><span class="default">$h </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">width&#xA0; </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">clamp</span><span class="keyword">(</span><span class="default">$w</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">height </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">clamp</span><span class="keyword">(</span><span class="default">$h</span><span class="keyword">);<br>&#xA0; }<br><br>&#xA0; public function </span><span class="default">__toString</span><span class="keyword">(){<br>&#xA0; &#xA0; return </span><span class="string">&quot;Dimension [width=</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">width</span><span class="string">, height=</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">height</span><span class="string">]&quot;</span><span class="keyword">;<br>&#xA0; }<br><br>&#xA0; protected static function </span><span class="default">clamp</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">){<br>&#xA0; &#xA0; if(</span><span class="default">$value </span><span class="keyword">&lt; </span><span class="default">self</span><span class="keyword">::</span><span class="default">MIN</span><span class="keyword">) </span><span class="default">$value </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">MIN</span><span class="keyword">;<br>&#xA0; &#xA0; if(</span><span class="default">$value </span><span class="keyword">&gt; </span><span class="default">self</span><span class="keyword">::</span><span class="default">MAX</span><span class="keyword">) </span><span class="default">$value </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">MAX</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; }<br>}<br><br>echo (new </span><span class="default">Dimension</span><span class="keyword">()) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>echo (new </span><span class="default">Dimension</span><span class="keyword">(</span><span class="default">1500</span><span class="keyword">, </span><span class="default">97</span><span class="keyword">)) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>echo (new </span><span class="default">Dimension</span><span class="keyword">(</span><span class="default">14</span><span class="keyword">, -</span><span class="default">20</span><span class="keyword">)) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>echo (new </span><span class="default">Dimension</span><span class="keyword">(</span><span class="default">240</span><span class="keyword">, </span><span class="default">80</span><span class="keyword">)) . </span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>- - - - - - - -<br> Dimension [width=0, height=0] - default size<br> Dimension [width=800, height=97] - width has been clamped to MAX<br> Dimension [width=14, height=0] - height has been clamped to MIN<br> Dimension [width=240, height=80] - width and height unchanged<br>- - - - - - - -<br><br>Setting upper and lower limits on your classes also help your objects make sense. For example, it is not possible for the width or height of a Dimension to be negative. It is up to you to keep phoney input from corrupting your objects, and to avoid potential errors and exceptions in other parts of your code.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.constants.php)

**[To root](/README.md)**