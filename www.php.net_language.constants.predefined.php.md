# Magic constants




<div class="phpcode"><span class="html">
the difference between <br>__FUNCTION__ and __METHOD__ as in PHP 5.0.4 is that<br><br>__FUNCTION__ returns only the name of the function<br><br>while as __METHOD__ returns the name of the class alongwith the name of the function<br><br>class trick<br>{<br>&#xA0; &#xA0; &#xA0; function doit()<br>&#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo __FUNCTION__;<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; function doitagain()<br>&#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo __METHOD__;<br>&#xA0; &#xA0; &#xA0; }<br>}<br>$obj=new trick();<br>$obj-&gt;doit();<br>output will be ----&#xA0; doit<br>$obj-&gt;doitagain();<br>output will be ----- trick::doitagain</span>
</div>
  

#


<div class="phpcode"><span class="html">
The __CLASS__ magic constant nicely complements the get_class() function.<br><br>Sometimes you need to know both:<br>- name of the inherited class<br>- name of the class actually executed<br><br>Here&apos;s an example that shows the possible solution:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">base_class<br></span><span class="keyword">{<br>&#xA0; &#xA0; function </span><span class="default">say_a</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&apos;a&apos; - said the &quot; </span><span class="keyword">. </span><span class="default">__CLASS__ </span><span class="keyword">. </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; function </span><span class="default">say_b</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&apos;b&apos; - said the &quot; </span><span class="keyword">. </span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">) . </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br>class </span><span class="default">derived_class </span><span class="keyword">extends </span><span class="default">base_class<br></span><span class="keyword">{<br>&#xA0; &#xA0; function </span><span class="default">say_a</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">say_a</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&apos;a&apos; - said the &quot; </span><span class="keyword">. </span><span class="default">__CLASS__ </span><span class="keyword">. </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; function </span><span class="default">say_b</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">say_b</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&apos;b&apos; - said the &quot; </span><span class="keyword">. </span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">) . </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$obj_b </span><span class="keyword">= new </span><span class="default">derived_class</span><span class="keyword">();<br><br></span><span class="default">$obj_b</span><span class="keyword">-&gt;</span><span class="default">say_a</span><span class="keyword">();<br>echo </span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">;<br></span><span class="default">$obj_b</span><span class="keyword">-&gt;</span><span class="default">say_b</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>The output should look roughly like this:<br><br>&apos;a&apos; - said the base_class<br>&apos;a&apos; - said the derived_class<br><br>&apos;b&apos; - said the derived_class<br>&apos;b&apos; - said the derived_class</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note a small inconsistency when using __CLASS__ and __METHOD__ in traits (stand php 7.0.4): While __CLASS__ is working as advertized and returns dynamically the name of the class the trait is being used in, __METHOD__ will actually prepend the trait name instead of the class name!</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is no way to implement a backwards compatible __DIR__ in versions prior to 5.3.0.<br><br>The only thing that you can do is to perform a recursive search and replace to dirname(__FILE__):<br>find . -type f -print0 | xargs -0 sed -i &apos;s/__DIR__/dirname(__FILE__)/&apos;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.constants.predefined.php)

**[To root](/)**