# Type Juggling




<div class="phpcode"><span class="html">
Uneven division of an integer variable by another integer variable will result in a float by automatic conversion -- you do not have to cast the variables to floats in order to avoid integer truncation (as you would in C, for example):<br><br>$dividend = 2;<br>$divisor = 3;<br>$quotient = $dividend/$divisor;<br>print $quotient; // 0.66666666666667</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;An example of PHP&apos;s automatic type conversion is the multiplication operator &apos;*&apos;. If either operand is a float, then both operands are evaluated as floats, and the result will be a float. Otherwise, the operands will be interpreted as integers, and the result will also be an integer. Note that this does not change the types of the operands themselves; the only change is in how the operands are evaluated and what the type of the expression itself is.&quot;<br><br>I understand what the doc is trying to say here, but this sentence is not correct as stated, other types can be coerced into floats.<br><br>e.g.<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="string">&quot;1.5&quot;</span><span class="keyword">; </span><span class="comment">// $a is a string<br></span><span class="default">$b </span><span class="keyword">= </span><span class="default">100</span><span class="keyword">; </span><span class="comment">// $b is an int<br></span><span class="default">$c </span><span class="keyword">= </span><span class="default">$a </span><span class="keyword">* </span><span class="default">$b</span><span class="keyword">; </span><span class="comment">// $c is a float, value is 150<br>// multiplication resulted in a float despite fact that neither operand was a float</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
incremental operator (&quot;++&quot;) doesn&apos;t make type conversion from boolean to int, and if an variable is boolean and equals TRUE than after ++ operation it remains as TRUE, so:<br><br>$a = TRUE; <br>echo ($a++).$a;&#xA0; // prints &quot;11&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
Casting objects to arrays is a pain. Example:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{<br><br>&#xA0; &#xA0; private </span><span class="default">$priv </span><span class="keyword">= </span><span class="string">&apos;priv_value&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; protected </span><span class="default">$prot </span><span class="keyword">= </span><span class="string">&apos;prot_value&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; public </span><span class="default">$pub </span><span class="keyword">= </span><span class="string">&apos;pub_value&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; public </span><span class="default">$MyClasspriv </span><span class="keyword">= </span><span class="string">&apos;second_pub_value&apos;</span><span class="keyword">;<br><br>}<br><br></span><span class="default">$test </span><span class="keyword">= new </span><span class="default">MyClass</span><span class="keyword">();<br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">((array) </span><span class="default">$test</span><span class="keyword">);<br><br></span><span class="comment">/*<br>Array<br>(<br>&#xA0; &#xA0; [MyClasspriv] =&gt; priv_value<br>&#xA0; &#xA0; [*prot] =&gt; prot_value<br>&#xA0; &#xA0; [pub] =&gt; pub_value<br>&#xA0; &#xA0; [MyClasspriv] =&gt; second_pub_value<br>)<br> */<br><br></span><span class="default">?&gt;<br></span><br>Yes, that looks like an array with two keys with the same name and it looks like the protected field was prepended with an asterisk. But that&apos;s not true:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">foreach ((array) </span><span class="default">$test </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$len </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$key</span><span class="keyword">}</span><span class="string"> (</span><span class="keyword">{</span><span class="default">$len</span><span class="keyword">}</span><span class="string">) =&gt; </span><span class="keyword">{</span><span class="default">$value</span><span class="keyword">}</span><span class="string">&lt;br /&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$len</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]) . </span><span class="string">&apos; &apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; echo </span><span class="string">&apos;&lt;hr /&gt;&apos;</span><span class="keyword">;<br>}<br><br></span><span class="comment">/*<br>MyClasspriv (13) =&gt; priv_value<br>0 77 121 67 108 97 115 115 0 112 114 105 118<br>*prot (7) =&gt; prot_value<br>0 42 0 112 114 111 116<br>pub (3) =&gt; pub_value<br>112 117 98<br>MyClasspriv (11) =&gt; second_pub_value<br>77 121 67 108 97 115 115 112 114 105 118<br> */<br><br></span><span class="default">?&gt;<br></span><br>The char codes show that the protected keys are prepended with &apos;\0*\0&apos; and private keys are prepended with &apos;\0&apos;.__CLASS__.&apos;\0&apos; so be careful when playing around with this.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Printing or echoing a FALSE boolean value or a NULL value results in an empty string:<br>(string)TRUE //returns &quot;1&quot;<br>(string)FALSE //returns &quot;&quot;<br>echo TRUE; //prints &quot;1&quot;<br>echo FALSE; //prints nothing!</span>
</div>
  

#


<div class="phpcode"><span class="html">
The object casting methods presented here do not take into account the class hierarchy of the class you&apos;re trying to cast your object into.<br><br>/**<br>&#xA0; &#xA0;&#xA0; * Convert an object to a specific class.<br>&#xA0; &#xA0;&#xA0; * @param object $object<br>&#xA0; &#xA0;&#xA0; * @param string $class_name The class to cast the object to<br>&#xA0; &#xA0;&#xA0; * @return object<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; public static function cast($object, $class_name) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if($object === false) return false;<br>&#xA0; &#xA0; &#xA0; &#xA0; if(class_exists($class_name)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ser_object&#xA0; &#xA0;&#xA0; = serialize($object);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $obj_name_len&#xA0; &#xA0;&#xA0; = strlen(get_class($object));<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $start&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = $obj_name_len + strlen($obj_name_len) + 6;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_object&#xA0; &#xA0; &#xA0; = &apos;O:&apos; . strlen($class_name) . &apos;:&quot;&apos; . $class_name . &apos;&quot;:&apos;;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_object&#xA0; &#xA0;&#xA0; .= substr($ser_object, $start);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_object&#xA0; &#xA0;&#xA0; = unserialize($new_object);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /**<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * The new object is of the correct type but<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * is not fully initialized throughout its graph.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * To get the full object graph (including parent<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * class data, we need to create a new instance of <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * the specified class and then assign the new <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; * properties to it.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $graph = new $class_name;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($new_object as $prop =&gt; $val) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $graph-&gt;$prop = $val;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $graph;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new CoreException(false, &quot;could not find class $class_name for casting in DB::cast&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }</span>
</div>
  

#


<div class="phpcode"><span class="html">
There are some shorter and faster (at least on my machine) ways to perform a type cast.<br><span class="default">&lt;?php<br>$string</span><span class="keyword">=</span><span class="string">&apos;12345.678&apos;</span><span class="keyword">;<br></span><span class="default">$float</span><span class="keyword">=+</span><span class="default">$string</span><span class="keyword">; <br></span><span class="default">$integer</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">|</span><span class="default">$string</span><span class="keyword">;<br></span><span class="default">$boolean</span><span class="keyword">=!!</span><span class="default">$string</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.type-juggling.php)

**[To root](/)**