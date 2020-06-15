# Examples




<div class="phpcode"><span class="html">
If you want to use method closures and don&apos;t have PHP 5.3, perhaps you find useful the following function
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">get_method_closure</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">,</span><span class="default">$method_name</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">),</span><span class="default">$method_name</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$func&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">create_function</span><span class="keyword">( </span><span class="string">&apos;&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $args&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; = func_get_args();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; static $object&#xA0; &#xA0; = NULL;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /*
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; * Check if this function is being called to set the static $object, which 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; * containts scope information to invoke the method
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; */
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(is_null($object) &amp;&amp; count($args) &amp;&amp; get_class($args[0])==&quot;&apos;</span><span class="keyword">.</span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">).</span><span class="string">&apos;&quot;){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $object = $args[0];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!is_null($object)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return call_user_func_array(array($object,&quot;&apos;</span><span class="keyword">.</span><span class="default">$method_name</span><span class="keyword">.</span><span class="string">&apos;&quot;),$args);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }else{
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return FALSE;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }&apos;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Initialize static $object
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$func</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Return closure
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$func</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;
<br></span>Here is how you would use it:
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">foo</span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">bar</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$foo </span><span class="keyword">= new </span><span class="default">foo</span><span class="keyword">();
<br></span><span class="default">$f </span><span class="keyword">= </span><span class="default">get_method_closure</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">,</span><span class="string">&apos;bar&apos;</span><span class="keyword">);
<br>echo </span><span class="default">$f</span><span class="keyword">(</span><span class="string">&quot;BAR&quot;</span><span class="keyword">);</span><span class="comment">//Prints &apos;bar&apos;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflection.examples.php)

**[To root](/README.md)**