# class_exists




<div class="phpcode"><span class="html">
If you are using aliasing to import namespaced classes, take care that class_exists will not work using the short, aliased class name - apparently whenever a class name is used as string, only the full-namespace version can be used<br><br>use a\namespaced\classname as coolclass;<br><br>class_exists( &apos;coolclass&apos; ) =&gt; false</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware: class_exists is case-INsensitive, as is class instantiation.<br><br>php &gt; var_dump(class_exists(&quot;DomNode&quot;));<br>bool(true)<br>php &gt; var_dump(class_exists(&quot;DOMNode&quot;));<br>bool(true)<br>php &gt; var_dump(class_exists(&quot;DOMNodE&quot;));<br>bool(true)<br>php &gt; $x = new DOMNOdE();<br>php &gt; var_dump(get_class($x));<br>string(7) &quot;DOMNode&quot;<br><br>(tested with PHP 5.5.10 on Linux)<br><br>This can cause some headaches in correlating class names to file names, especially on a case-sensitive file system.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you recursively load several classes inside an autoload function (or mix manual loading and autoloading), be aware that class_exists() (as well as get_declared_classes()) does not know about classes previously loaded during the *current* autoload invocation.<br><br>Apparently, the internal list of declared classes is only updated after the autoload function is completed.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi guys!<br>Be careful&#xA0; and don&apos;t forget about second boolean argument $autoload (TRUE by default) when check exists class after spl_autoload_register. Propose short example<br>file second.php<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Second </span><span class="keyword">{}<br></span><span class="default">?&gt;<br></span>file index.php<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">First<br></span><span class="keyword">{<br>&#xA0; &#xA0; function </span><span class="default">first</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$bool</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">spl_autoload_register</span><span class="keyword">( function(</span><span class="default">$class</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; require </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">) . </span><span class="string">&apos;.php&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; });<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">class_exists</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$bool</span><span class="keyword">)?</span><span class="string">&apos;Exist!!!!&apos;</span><span class="keyword">:</span><span class="string">&apos;Not exist!&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>new </span><span class="default">First</span><span class="keyword">(</span><span class="default">$class </span><span class="keyword">= </span><span class="string">&apos;Second&apos;</span><span class="keyword">, </span><span class="default">$bool </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">); </span><span class="comment">//Exist!!!!<br></span><span class="keyword">new </span><span class="default">First</span><span class="keyword">(</span><span class="default">$class </span><span class="keyword">= </span><span class="string">&apos;Second&apos;</span><span class="keyword">, </span><span class="default">$bool </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">); </span><span class="comment">//Not exist!<br></span><span class="default">?&gt;<br></span>Because __autoload executing much earlier than boolean returned, imho..</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.class-exists.php)

**[To root](/README.md)**