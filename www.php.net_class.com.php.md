# The com class




<div class="phpcode"><span class="html">
Took me a while to figure this out by trial and error:<br><br>If your frustrated when getting a com-exception like &quot;Error [0x80004002] ...&quot; (with a message nagging you about an unsupported interface), and you&apos;re calling a class method through COM that expects a char as an argument, you should NOT call the method with a single-character string.<br><br>The correct way of calling a method like this is to use a VARIANT with a type of VT_UI1.<br><br>COM enabled method definition: <br><br>public bool DoSomething(char arg);<br><br><span class="default">&lt;?php<br><br>$com </span><span class="keyword">= new </span><span class="default">COM</span><span class="keyword">(</span><span class="string">&apos;Some.Class.Name&apos;</span><span class="keyword">);<br><br></span><span class="comment">// This will fail<br></span><span class="default">$var </span><span class="keyword">= </span><span class="string">&apos;a&apos;</span><span class="keyword">;<br></span><span class="default">$com</span><span class="keyword">-&gt;</span><span class="default">DoSomething</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">);<br><br></span><span class="comment">// This works correctly<br></span><span class="default">$var </span><span class="keyword">= new </span><span class="default">VARIANT</span><span class="keyword">(</span><span class="default">ord</span><span class="keyword">(</span><span class="string">&apos;a&apos;</span><span class="keyword">), </span><span class="default">VT_UI1</span><span class="keyword">);<br></span><span class="default">$com</span><span class="keyword">-&gt;</span><span class="default">DoSomething</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>I hope this helps some people</span>
</div>
  

#


<div class="phpcode"><span class="html">
After one week of trying to understand what was wrong with my PHP communication with MS Word, i finally got it working...<br>It seems that if you&apos;re running IIS, the COM object are invoked with restricted privileges.<br>If you&apos;re having permission problems like not being able to opening or saving a document and you&apos;re getting errors like:<br><br> - This command is not available because no document is open<br>or<br> - Command failed<br><br>try this (if you&apos;re running IIS):<br><br>- Execute &quot;dcomcnfg&quot;<br>- Open Component Services &gt; Computers &gt; My Computer &gt; DCOM Config<br>- Search for Microsoft Office Word 97-2003 Document (it will be something like this translated to your language, so take a while and search for it)<br>- Right-Click on it and open the properties<br>- Choose &quot;Identity&quot; tab<br>- Normally this is set to &quot;the launching user&quot;. You have to change this to &quot;the interactive user&quot; or a admin user of your choice.<br>- Apply these new settings and test your COM application. It should work fine now.<br><br>I hope I save lots and lots of hours of headaches to some of you :)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.com.php)

**[⬆ to root](/)**