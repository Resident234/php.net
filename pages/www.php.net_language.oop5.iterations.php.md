# Object Iteration





there is still an open bug about using current() etc. with iterators
https://bugs.php.net/bug.php?id=49369

  

#



By reading the posts below I wondered if it really is impossible to make an ArrayAccess implementation really behave like a true array ( by being multi level )

Seems like it&apos;s not impossible. Not very preety but usable



```
<?php

class ArrayAccessImpl implements ArrayAccess {

&#xA0; private $data = array();

&#xA0; public function offsetUnset($index) {}

&#xA0; public function offsetSet($index, $value) {
//&#xA0; &#xA0; echo (&quot;SET: &quot;.$index.&quot;&lt;br&gt;&quot;);
&#xA0; &#xA0; 
&#xA0; &#xA0; if(isset($data[$index])) {
&#xA0; &#xA0; &#xA0; &#xA0; unset($data[$index]);
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; $u = &amp;$this-&gt;data[$index];
&#xA0; &#xA0; if(is_array($value)) {
&#xA0; &#xA0; &#xA0; &#xA0; $u = new ArrayAccessImpl();
&#xA0; &#xA0; &#xA0; &#xA0; foreach($value as $idx=&gt;$e)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $u[$idx]=$e;
&#xA0; &#xA0; } else
&#xA0; &#xA0; &#xA0; &#xA0; $u=$value;
&#xA0; }

&#xA0; public function offsetGet($index) {
//&#xA0; &#xA0; echo (&quot;GET: &quot;.$index.&quot;&lt;br&gt;&quot;);

&#xA0; &#xA0; if(!isset($this-&gt;data[$index]))
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;data[$index]=new ArrayAccessImpl();
&#xA0; &#xA0; 
&#xA0; &#xA0; return $this-&gt;data[$index];
&#xA0; }

&#xA0; public function offsetExists($index) {
//&#xA0; &#xA0; echo (&quot;EXISTS: &quot;.$index.&quot;&lt;br&gt;&quot;);
&#xA0; &#xA0; 
&#xA0; &#xA0; if(isset($this-&gt;data[$index])) {
&#xA0; &#xA0; &#xA0; &#xA0; if($this-&gt;data[$index] instanceof ArrayAccessImpl) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(count($this-&gt;data[$index]-&gt;data)&gt;0)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; } else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; } else
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; }

}

echo &quot;ArrayAccess implementation that behaves like a multi-level array&lt;hr /&gt;&quot;;

$data = new ArrayAccessImpl();

$data[&apos;string&apos;]=&quot;Just a simple string&quot;;
$data[&apos;number&apos;]=33;
$data[&apos;array&apos;][&apos;another_string&apos;]=&quot;Alpha&quot;;
$data[&apos;array&apos;][&apos;some_object&apos;]=new stdClass();
$data[&apos;array&apos;][&apos;another_array&apos;][&apos;x&apos;][&apos;y&apos;]=&quot;LOL @ Whoever said it can&apos;t be done !&quot;;
$data[&apos;blank_array&apos;]=array();

echo &quot;&apos;array&apos; Isset? &quot;; print_r(isset($data[&apos;array&apos;])); echo &quot;&lt;hr /&gt;&quot;;
echo &quot;&lt;pre&gt;&quot;; print_r($data[&apos;array&apos;][&apos;non_existent&apos;]); echo &quot;&lt;/pre&gt;If attempting to read an offset that doesn&apos;t exist it returns a blank object! Use isset() to check if it exists!&lt;br&gt;&quot;;
echo &quot;&apos;non_existent&apos; Isset? &quot;; print_r(isset($data[&apos;array&apos;][&apos;non_existent&apos;])); echo &quot;&lt;br /&gt;&quot;;
echo &quot;&lt;pre&gt;&quot;; print_r($data[&apos;blank_array&apos;]); echo &quot;&lt;/pre&gt;A blank array unfortunately returns similar results :(&lt;br /&gt;&quot;;
echo &quot;&apos;blank_array&apos; Isset? &quot;; print_r(isset($data[&apos;blank_array&apos;])); echo &quot;&lt;hr /&gt;&quot;;
echo &quot;&lt;pre&gt;&quot;; print_r($data); echo &quot;&lt;/pre&gt; (non_existent remains in the structure. If someone can help to solve this I&apos;ll appreciate it)&lt;hr /&gt;&quot;;

echo &quot;Display some value that exists: &quot;.$data[&apos;array&apos;][&apos;another_string&apos;];

?>
```


(in the two links mentioned below by artur at jedlinski... they say you can&apos;t use references, so I didn&apos;t used them.
My implementation uses recursive objects)

If anyone finds a better (cleaner) sollution, please e-mail me.
Thanks,
Wave.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.iterations.php)

**[To root](/README.md)**