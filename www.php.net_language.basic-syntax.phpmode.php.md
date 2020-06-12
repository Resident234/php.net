# Escaping from HTML




<div class="phpcode"><span class="html">
When the documentation says that the PHP parser ignores everything outside the <span class="default">&lt;?php </span><span class="keyword">... </span><span class="default">?&gt;</span> tags, it means literally EVERYTHING. Including things you normally wouldn&apos;t consider &quot;valid&quot;, such as the following:<br><br>&lt;html&gt;&lt;body&gt;<br>&lt;p<span class="default">&lt;?php </span><span class="keyword">if (</span><span class="default">$highlight</span><span class="keyword">): </span><span class="default">?&gt;</span> class=&quot;highlight&quot;<span class="default">&lt;?php </span><span class="keyword">endif;</span><span class="default">?&gt;</span>&gt;This is a paragraph.&lt;/p&gt;<br>&lt;/body&gt;&lt;/html&gt;<br><br>Notice how the PHP code is embedded in the middle of an HTML opening tag. The PHP parser doesn&apos;t care that it&apos;s in the middle of an opening tag, and doesn&apos;t require that it be closed. It also doesn&apos;t care that after the closing ?&gt; tag is the end of the HTML opening tag. So, if $highlight is true, then the output will be:<br><br>&lt;html&gt;&lt;body&gt;<br>&lt;p class=&quot;highlight&quot;&gt;This is a paragraph.&lt;/p&gt;<br>&lt;/body&gt;&lt;/html&gt;<br><br>Otherwise, it will be:<br><br>&lt;html&gt;&lt;body&gt;<br>&lt;p&gt;This is a paragraph.&lt;/p&gt;<br>&lt;/body&gt;&lt;/html&gt;<br><br>Using this method, you can have HTML tags with optional attributes, depending on some PHP condition. Extremely flexible and useful!</span>
</div>
  

#


<div class="phpcode"><span class="html">
One aspect of PHP that you need to be careful of, is that ?&gt; will drop you out of PHP code and into HTML even if it appears inside a // comment. (This does not apply to /* */ comments.) This can lead to unexpected results. For example, take this line:<br><br><span class="default">&lt;?php<br>&#xA0; $file_contents&#xA0; </span><span class="keyword">= </span><span class="string">&apos;&lt;?php die(); ?&gt;&apos; </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>If you try to remove it by turning it into a comment, you get this:<br><br><span class="default">&lt;?php<br></span><span class="comment">//&#xA0; $file_contents&#xA0; = &apos;&lt;?php die(); </span><span class="default">?&gt;</span>&apos; . &quot;\n&quot;;<br>?&gt;<br><br>Which results in &apos; . &quot;\n&quot;; (and whatever is in the lines following it) to be output to your HTML page.<br><br>The cure is to either comment it out using /* */ tags, or re-write the line as:<br><br><span class="default">&lt;?php<br>&#xA0; $file_contents&#xA0; </span><span class="keyword">= </span><span class="string">&apos;&lt;&apos; </span><span class="keyword">. </span><span class="string">&apos;?php die(); ?&apos; </span><span class="keyword">. </span><span class="string">&apos;&gt;&apos; </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Although not specifically pointed out in the main text, escaping from HTML also applies to other control statements:<br><br><span class="default">&lt;?php </span><span class="keyword">for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">5</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">): </span><span class="default">?&gt;<br></span>Hello, there!<br><span class="default">&lt;?php </span><span class="keyword">endfor; </span><span class="default">?&gt;<br></span><br>When the above code snippet is executed we get the following output:<br><br>Hello, there!<br>Hello, there!<br>Hello, there!<br>Hello, there!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Playing around with different open and close tags I discovered you can actually mix different style open/close tags<br><br>some examples<br><br>&lt;%<br>//your php code here<br>?&gt;<br><br>or<br><br>&lt;script language=&quot;php&quot;&gt;<br>//php code here<br>%&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.phpmode.php)

**[⬆ to root](/)**