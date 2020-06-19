# Variable variables




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br>&#xA0; </span><span class="comment">//You can even add more Dollar Signs<br><br>&#xA0; </span><span class="default">$Bar </span><span class="keyword">= </span><span class="string">&quot;a&quot;</span><span class="keyword">;<br>&#xA0; </span><span class="default">$Foo </span><span class="keyword">= </span><span class="string">&quot;Bar&quot;</span><span class="keyword">;<br>&#xA0; </span><span class="default">$World </span><span class="keyword">= </span><span class="string">&quot;Foo&quot;</span><span class="keyword">;<br>&#xA0; </span><span class="default">$Hello </span><span class="keyword">= </span><span class="string">&quot;World&quot;</span><span class="keyword">;<br>&#xA0; </span><span class="default">$a </span><span class="keyword">= </span><span class="string">&quot;Hello&quot;</span><span class="keyword">;<br><br>&#xA0; </span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns Hello<br>&#xA0; </span><span class="keyword">$</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns World<br>&#xA0; </span><span class="keyword">$$</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns Foo<br>&#xA0; </span><span class="keyword">$$$</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns Bar<br>&#xA0; </span><span class="keyword">$$$$</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns a<br><br>&#xA0; </span><span class="keyword">$$$$$</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns Hello<br>&#xA0; </span><span class="keyword">$$$$$$</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">//Returns World<br><br>&#xA0; //... and so on ...//<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It may be worth specifically noting, if variable names follow some kind of &quot;template,&quot; they can be referenced like this:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Given these variables ...<br></span><span class="default">$nameTypes&#xA0; &#xA0; </span><span class="keyword">= array(</span><span class="string">&quot;first&quot;</span><span class="keyword">, </span><span class="string">&quot;last&quot;</span><span class="keyword">, </span><span class="string">&quot;company&quot;</span><span class="keyword">);<br></span><span class="default">$name_first&#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&quot;John&quot;</span><span class="keyword">;<br></span><span class="default">$name_last&#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&quot;Doe&quot;</span><span class="keyword">;<br></span><span class="default">$name_company </span><span class="keyword">= </span><span class="string">&quot;PHP.net&quot;</span><span class="keyword">;<br><br></span><span class="comment">// Then this loop is ...<br></span><span class="keyword">foreach(</span><span class="default">$nameTypes </span><span class="keyword">as </span><span class="default">$type</span><span class="keyword">)<br>&#xA0; print ${</span><span class="string">&quot;name_</span><span class="default">$type</span><span class="string">&quot;</span><span class="keyword">} . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="comment">// ... equivalent to this print statement.<br></span><span class="keyword">print </span><span class="string">&quot;</span><span class="default">$name_first</span><span class="string">\n</span><span class="default">$name_last</span><span class="string">\n</span><span class="default">$name_company</span><span class="string">\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>This is apparent from the notes others have left, but is not explicitly stated.</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">// $variable-name = &apos;parse error&apos;;<br>// You can&apos;t do that but you can do this:<br></span><span class="default">$a </span><span class="keyword">= </span><span class="string">&apos;variable-name&apos;</span><span class="keyword">;<br>$</span><span class="default">$a </span><span class="keyword">= </span><span class="string">&apos;hello&apos;</span><span class="keyword">;<br>echo </span><span class="default">$variable</span><span class="keyword">-</span><span class="default">name </span><span class="keyword">. </span><span class="string">&apos; &apos; </span><span class="keyword">. $</span><span class="default">$a</span><span class="keyword">; </span><span class="comment">// Gives&#xA0; &#xA0;&#xA0; 0 hello<br></span><span class="default">?&gt;<br></span><br>For a particular reason I had been using some variable names with hyphens for ages. There was no problem because they were only referenced via a variable variable. I only saw a parse error much later, when I tried to reference one directly. It took a while to realise that illegal hyphens were the cause because the parse error only occurs on assignment.</span>
</div>
  

#


<div class="phpcode"><span class="html">
PHP actually supports invoking a new instance of a class using a variable class name since at least version 5.2<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0;&#xA0; public function </span><span class="default">hello</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Hello world!&apos;</span><span class="keyword">;<br>&#xA0;&#xA0; }<br>}<br></span><span class="default">$my_foo </span><span class="keyword">= </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;<br></span><span class="default">$a </span><span class="keyword">= new </span><span class="default">$my_foo</span><span class="keyword">();<br></span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">hello</span><span class="keyword">(); </span><span class="comment">//prints &apos;Hello world!&apos;<br></span><span class="default">?&gt;<br></span><br>Additionally, you can access static methods and properties using variable class names, but only since PHP 5.3<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0;&#xA0; public static function </span><span class="default">hello</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Hello world!&apos;</span><span class="keyword">;<br>&#xA0;&#xA0; }<br>}<br></span><span class="default">$my_foo </span><span class="keyword">= </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;<br></span><span class="default">$my_foo</span><span class="keyword">::</span><span class="default">hello</span><span class="keyword">(); </span><span class="comment">//prints &apos;Hello world!&apos;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You may think of using variable variables to dynamically generate variables from an array, by doing something similar to: -<br><br><span class="default">&lt;?php<br> </span><span class="keyword">foreach (</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) <br> {<br>&#xA0; $</span><span class="default">$key</span><span class="keyword">= </span><span class="default">$value</span><span class="keyword">;<br> }<br><br></span><span class="default">?&gt;<br></span><br>This however would be reinventing the wheel when you can simply use: <br><br><span class="default">&lt;?php<br>extract</span><span class="keyword">( </span><span class="default">$array</span><span class="keyword">, </span><span class="default">EXTR_OVERWRITE</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Note that this will overwrite the contents of variables that already exist.<br><br>Extract has useful functionality to prevent this, or you may group the variables by using prefixes too, so you could use: -<br><br>EXTR_PREFIX_ALL<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">=array(</span><span class="string">&quot;one&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;First Value&quot;</span><span class="keyword">,<br></span><span class="string">&quot;two&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;2nd Value&quot;</span><span class="keyword">,<br></span><span class="string">&quot;three&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;8&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br></span><span class="default">extract</span><span class="keyword">( </span><span class="default">$array</span><span class="keyword">, </span><span class="default">EXTR_PREFIX_ALL</span><span class="keyword">, </span><span class="string">&quot;my_prefix_&quot;</span><span class="keyword">);<br>&#xA0;&#xA0; <br></span><span class="default">?&gt;<br></span><br>This would create variables: -<br>$my_prefix_one <br>$my_prefix_two<br>$my_prefix_three<br><br>containing: -<br>&quot;First Value&quot;, &quot;2nd Value&quot; and &quot;8&quot; respectively</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to use a variable value in part of the name of a variable variable (not the whole name itself), you can do like the following:<br><br><span class="default">&lt;?php<br>$price_for_monday </span><span class="keyword">= </span><span class="default">10</span><span class="keyword">;<br></span><span class="default">$price_for_tuesday </span><span class="keyword">= </span><span class="default">20</span><span class="keyword">;<br></span><span class="default">$price_for_wednesday </span><span class="keyword">= </span><span class="default">30</span><span class="keyword">;<br><br></span><span class="default">$today </span><span class="keyword">= </span><span class="string">&apos;tuesday&apos;</span><span class="keyword">;<br><br></span><span class="default">$price_for_today </span><span class="keyword">= ${ </span><span class="string">&apos;price_for_&apos; </span><span class="keyword">. </span><span class="default">$today</span><span class="keyword">};<br>echo </span><span class="default">$price_for_today</span><span class="keyword">; </span><span class="comment">// will return 20<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Another use for this feature in PHP is dynamic parsing..&#xA0; 
<br>
<br>Due to the rather odd structure of an input string I am currently parsing, I must have a reference for each particular object instantiation in the order which they were created.&#xA0; In addition, because of the syntax of the input string, elements of the previous object creation are required for the current one.&#xA0; 
<br>
<br>Normally, you won&apos;t need something this convolute.&#xA0; In this example, I needed to load an array with dynamically named objects - (yes, this has some basic Object Oriented programming, please bare with me..)
<br>
<br><span class="default">&lt;?php
<br>&#xA0;&#xA0; </span><span class="keyword">include(</span><span class="string">&quot;obj.class&quot;</span><span class="keyword">);
<br>
<br>&#xA0;&#xA0; </span><span class="comment">// this is only a skeletal example, of course.
<br>&#xA0;&#xA0; </span><span class="default">$object_array </span><span class="keyword">= array();
<br>
<br>&#xA0;&#xA0; </span><span class="comment">// assume the $input array has tokens for parsing.
<br>&#xA0;&#xA0; </span><span class="keyword">foreach (</span><span class="default">$input_array </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// test to ensure the $value is what we need.
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$obj </span><span class="keyword">= </span><span class="string">&quot;obj&quot;</span><span class="keyword">.</span><span class="default">$key</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $</span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">Obj</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$other_var</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">Array_Push</span><span class="keyword">(</span><span class="default">$object_array</span><span class="keyword">, $</span><span class="default">$obj</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="comment">// etc..
<br>&#xA0;&#xA0; </span><span class="keyword">}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Now, we can use basic array manipulation to get these objects out in the particular order we need, and the objects no longer are dependant on the previous ones.
<br>
<br>I haven&apos;t fully tested the implimentation of the objects.&#xA0; The&#xA0; scope of a variable-variable&apos;s object attributes (get all that?) is a little tough to crack.&#xA0; Regardless, this is another example of the manner in which the var-vars can be used with precision where tedious, extra hard-coding is the only alternative.
<br>
<br>Then, we can easily pull everything back out again using a basic array function: foreach.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">//...
<br>&#xA0;&#xA0; </span><span class="keyword">foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$object</span><span class="keyword">){
<br>
<br>&#xA0; &#xA0; &#xA0; echo </span><span class="default">$key</span><span class="keyword">.</span><span class="string">&quot; -- &quot;</span><span class="keyword">.</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">print_fcn</span><span class="keyword">().</span><span class="string">&quot; &lt;br/&gt;\n&quot;</span><span class="keyword">;
<br>
<br>&#xA0;&#xA0; } </span><span class="comment">// end foreach&#xA0;&#xA0; 
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Through this, we can pull a dynamically named object out of the array it was stored in without actually knowing its name.</span>
</div>
  

#


<div class="phpcode"><span class="html">
While not relevant in everyday PHP programming, it seems to be possible to insert whitespace and comments between the dollar signs of a variable variable.&#xA0; All three comment styles work. This information becomes relevant when writing a parser, tokenizer or something else that operates on PHP syntax.<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; $foo </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; $<br><br>&#xA0; &#xA0; </span><span class="comment">/*<br>&#xA0; &#xA0; &#xA0; &#xA0; I am complete legal and will compile without notices or error as a variable variable.<br>&#xA0; &#xA0; */<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$foo </span><span class="keyword">= </span><span class="string">&apos;magic&apos;</span><span class="keyword">;<br><br>&#xA0; &#xA0; echo </span><span class="default">$bar</span><span class="keyword">; </span><span class="comment">// Outputs magic.<br><br></span><span class="default">?&gt;<br></span><br>Behaviour tested with PHP Version 5.6.19</span>
</div>
  

#


<div class="phpcode"><span class="html">
Adding an element directly to an array using variables:<br><br><span class="default">&lt;?php<br>$tab </span><span class="keyword">= array(</span><span class="string">&quot;one&quot;</span><span class="keyword">, </span><span class="string">&quot;two&quot;</span><span class="keyword">, </span><span class="string">&quot;three&quot;</span><span class="keyword">) ;<br></span><span class="default">$a </span><span class="keyword">= </span><span class="string">&quot;tab&quot; </span><span class="keyword">;<br>$</span><span class="default">$a</span><span class="keyword">[] =</span><span class="string">&quot;four&quot; </span><span class="keyword">; </span><span class="comment">// &lt;==== fatal error<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$tab</span><span class="keyword">) ;<br></span><span class="default">?&gt;<br></span>will issue this error:<br><br>Fatal error: Cannot use [] for reading<br><br>This is not a bug, you need to use the {} syntax to remove the ambiguity.<br><br><span class="default">&lt;?php<br>$tab </span><span class="keyword">= array(</span><span class="string">&quot;one&quot;</span><span class="keyword">, </span><span class="string">&quot;two&quot;</span><span class="keyword">, </span><span class="string">&quot;three&quot;</span><span class="keyword">) ;<br></span><span class="default">$a </span><span class="keyword">= </span><span class="string">&quot;tab&quot; </span><span class="keyword">;<br>${</span><span class="default">$a</span><span class="keyword">}[] =&#xA0; </span><span class="string">&quot;four&quot; </span><span class="keyword">; </span><span class="comment">// &lt;==== this is the correct way to do it<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$tab</span><span class="keyword">) ;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.variable.php)

**[To root](/README.md)**