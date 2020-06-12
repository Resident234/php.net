# unserialize




<div class="phpcode"><span class="html">
Just some reminder which may save somebody some time regarding the `$options` array: <br><br>Say you want to be on the safe side and not allow any objects to be unserialized... My first thought was doing the following:<br><br><span class="default">&lt;?php<br>$lol </span><span class="keyword">= </span><span class="default">unserialize</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);<br></span><span class="comment">// This will generate:<br>// Warning: unserialize() expects parameter 2 to be array, boolean given<br></span><span class="default">?&gt;<br></span><br>The correct way of doing this is the following:<br><span class="default">&lt;?php<br>$lol </span><span class="keyword">= </span><span class="default">unserialize</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, [</span><span class="string">&apos;allowed_classes&apos; </span><span class="keyword">=&gt; </span><span class="default">false</span><span class="keyword">]);<br></span><span class="default">?&gt;<br></span><br>Hope it helps somebody!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a note - if the serialized string contains a reference to a class that cannot be instantiated (e.g. being abstract) PHP will immediately die with a fatal error. If the unserialize() statement is preceded with a &apos;@&apos; to avoid cluttering the logs with warns or notices there will be absolutely no clue as to why the script stopped working. Cost me a couple of hours...</span>
</div>
  

#


<div class="phpcode"><span class="html">
In the Classes and Objects docs, there is this: In order to be able to unserialize() an object, the class of that object needs to be defined.<br><br>Prior to PHP 5.3, this was not an issue.&#xA0; But after PHP 5.3 an object made by SimpleXML_Load_String() cannot be serialized.&#xA0; An attempt to do so will result in a run-time failure, throwing an exception.&#xA0; If you store such an object in $_SESSION, you will get a post-execution error that says this:<br><br>Fatal error: Uncaught exception &apos;Exception&apos; with message &apos;Serialization of &apos;SimpleXMLElement&apos; is not allowed&apos; in [no active file]:0 Stack trace: #0 {main} thrown in [no active file] on line 0<br><br>The entire contents of the session will be lost.&#xA0; Hope this saves someone some time!<br><br><span class="default">&lt;?php </span><span class="comment">// RAY_temp_ser.php<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);<br></span><span class="default">session_start</span><span class="keyword">();<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$_SESSION</span><span class="keyword">);<br></span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;hello&apos;</span><span class="keyword">] = </span><span class="string">&apos;World&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$_SESSION</span><span class="keyword">);<br><br></span><span class="comment">// AN XML STRING FOR TEST DATA<br></span><span class="default">$xml </span><span class="keyword">= </span><span class="string">&apos;&lt;?xml version=&quot;1.0&quot;?&gt;<br>&lt;families&gt;<br>&#xA0; &lt;parent&gt;<br>&#xA0; &#xA0; &lt;child index=&quot;1&quot; value=&quot;Category 1&quot;&gt;Child One&lt;/child&gt;<br>&#xA0; &lt;/parent&gt;<br>&lt;/families&gt;&apos;</span><span class="keyword">;<br><br></span><span class="comment">// MAKE AN OBJECT (GIVES SimpleXMLElement)<br></span><span class="default">$obj </span><span class="keyword">= </span><span class="default">SimpleXML_Load_String</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);<br><br></span><span class="comment">// STORE THE OBJECT IN THE SESSION<br></span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;obj&apos;</span><span class="keyword">] = </span><span class="default">$obj</span><span class="keyword">;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.unserialize.php)

**[⬆ to root](/)**