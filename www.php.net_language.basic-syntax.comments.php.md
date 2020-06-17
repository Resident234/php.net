# Comments




<div class="phpcode"><span class="html">
Notes can come in all sorts of shapes and sizes. They vary, and their uses are completely up to the person writing the code. However, I try to keep things consistent in my code that way it&apos;s easy for the next person to read. So something like this might help...<br><br><span class="default">&lt;?php<br><br></span><span class="comment">//======================================================================<br>// CATEGORY LARGE FONT<br>//======================================================================<br><br>//-----------------------------------------------------<br>// Sub-Category Smaller Font<br>//-----------------------------------------------------<br><br>/* Title Here Notice the First Letters are Capitalized */<br><br># Option 1<br># Option 2<br># Option 3<br><br>/*<br> * This is a detailed explanation<br> * of something that should require<br> * several paragraphs of information.<br> */<br> <br>// This is a single line quote.<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A nice way to toggle the commenting of blocks of code can be done by mixing the two comment styles:<br><span class="default">&lt;?php<br></span><span class="comment">//*<br></span><span class="keyword">if (</span><span class="default">$foo</span><span class="keyword">) {<br>&#xA0; echo </span><span class="default">$bar</span><span class="keyword">;<br>}<br></span><span class="comment">// */<br></span><span class="default">sort</span><span class="keyword">(</span><span class="default">$morecode</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Now by taking out one / on the first line..<br><br><span class="default">&lt;?php<br></span><span class="comment">/*<br>if ($foo) {<br>&#xA0; echo $bar;<br>}<br>// */<br></span><span class="default">sort</span><span class="keyword">(</span><span class="default">$morecode</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>..the block is suddenly commented out.<br>This works because a /* .. */ overrides //. You can even &quot;flip&quot; two blocks, like this:<br><span class="default">&lt;?php<br></span><span class="comment">//*<br></span><span class="keyword">if (</span><span class="default">$foo</span><span class="keyword">) {<br>&#xA0; echo </span><span class="default">$bar</span><span class="keyword">;<br>}<br></span><span class="comment">/*/<br>if ($bar) {<br>&#xA0; echo $foo;<br>}<br>// */<br></span><span class="default">?&gt;<br></span>vs<br><span class="default">&lt;?php<br></span><span class="comment">/*<br>if ($foo) {<br>&#xA0; echo $bar;<br>}<br>/*/<br></span><span class="keyword">if (</span><span class="default">$bar</span><span class="keyword">) {<br>&#xA0; echo </span><span class="default">$foo</span><span class="keyword">;<br>}<br></span><span class="comment">// */<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is worth mentioning that, HTML comments have no meaning in PHP parser. So,<br><br>&lt;!-- comment<br><span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">some_function</span><span class="keyword">(); </span><span class="default">?&gt;<br></span>--&gt;<br><br>WILL execute some_function() and echo result inside HTML comment.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Comments in PHP can be used for several purposes, a very interesting one being that you can generate API documentation directly from them by using PHPDocumentor (<a href="http://www.phpdoc.org/" rel="nofollow" target="_blank">http://www.phpdoc.org/</a>).<br><br>Therefor one has to use a JavaDoc-like comment syntax (conforms to the DocBook DTD), example:<br><span class="default">&lt;?php<br></span><span class="comment">/**<br>* The second * here opens the DocBook commentblock, which could later on&lt;br&gt;<br>* in your development cycle save you a lot of time by preventing you having to rewrite&lt;br&gt;<br>* major documentation parts to generate some usable form of documentation.<br>*/<br></span><span class="default">?&gt;<br></span>Some basic html-like formatting is supported with this (ie &lt;br&gt; tags) to create something of a layout.</span>
</div>
  

#


<div class="phpcode"><span class="html">
MSpreij (8-May-2005) says&#xA0; /* .. */ overrides //&#xA0; <br>Anonymous (26-Jan-2006) says // overrides /* .. */<br><br>Actually, both are correct. Once a comment is opened, *everything* is ignored until the end of the comment (or the end of the php block) is reached.<br><br>Thus, if a comment is opened with: <br>&#xA0;&#xA0; //&#xA0; then /* and */ are &quot;overridden&quot; until after end-of-line <br>&#xA0;&#xA0; /*&#xA0; then // is &quot;overridden&quot; until after */</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful when commenting out regular expressions.<br><br>E.g. the following causes a parser error.<br><br>I do prefer using # as regexp delimiter anyway so it won&apos;t hurt me ;-)<br><br><span class="default">&lt;?php <br><br></span><span class="comment">/*<br><br> $f-&gt;setPattern(&apos;/^\d.*/</span><span class="string">&apos;);<br><br>*/<br><br>?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
it&apos;s perhaps not obvious to some, but the following code will cause a parse error! the ?&gt; in //?&gt; is not treated as commented text, this is a result of having to handle code on one line such as <span class="default">&lt;?php </span><span class="keyword">echo </span><span class="string">&apos;something&apos;</span><span class="keyword">; </span><span class="comment">//comment </span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="keyword">if(</span><span class="default">1</span><span class="keyword">==</span><span class="default">1</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">//</span><span class="default">?&gt;<br></span>}<br>?&gt;<br><br>i discovered this &quot;anomally&quot; when i commented out a line of code containing a regex which itself contained ?&gt;, with the // style comment.<br>e.g. //preg_match(&apos;/^(?&gt;c|b)at$/&apos;, &apos;cat&apos;, $matches);<br>will cause an error while commented! using /**/ style comments provides a solution. i don&apos;t know about # style comments, i don&apos;t ever personally use them.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Comments do NOT take up processing power.<br><br>So, for all the people who argue that comments are undesired because they take up processing power now have no reason to comment ;)<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// Control<br></span><span class="keyword">echo </span><span class="default">microtime</span><span class="keyword">(), </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">; </span><span class="comment">// 0.25163600 1292450508<br></span><span class="keyword">echo </span><span class="default">microtime</span><span class="keyword">(), </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">; </span><span class="comment">// 0.25186000 1292450508<br><br>// Test<br></span><span class="keyword">echo </span><span class="default">microtime</span><span class="keyword">(), </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">; </span><span class="comment">// 0.25189700 1292450508<br># TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST<br># .. Above comment repeated 18809 times ..<br></span><span class="keyword">echo </span><span class="default">microtime</span><span class="keyword">(), </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">; </span><span class="comment">// 0.25192100 1292450508<br><br></span><span class="default">?&gt;<br></span><br>They take up about the same amount of time (about meaning on a repeated testing, sometimes the difference between the control and the test was negative and sometimes positive).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.comments.php)

**[To root](/README.md)**