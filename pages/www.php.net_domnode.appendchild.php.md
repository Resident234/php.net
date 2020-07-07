# DOMNode::appendChild




<div class="phpcode"><span class="html">
What&apos;s not mentioned here is that DOMNode::appendChild() can also be used to move an existing node to another part of the DOMDocument, e.g.<br><br><span class="default">&lt;?php<br>$doc </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">();<br></span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">loadXML</span><span class="keyword">(</span><span class="string">&quot;&lt;foobar&gt;&lt;bar/&gt;&lt;foo/&gt;&lt;/foobar&gt;&quot;</span><span class="keyword">);<br></span><span class="default">$bar </span><span class="keyword">= </span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">documentElement</span><span class="keyword">-&gt;</span><span class="default">firstChild</span><span class="keyword">;<br></span><span class="default">$foo </span><span class="keyword">= </span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">documentElement</span><span class="keyword">-&gt;</span><span class="default">lastChild</span><span class="keyword">;<br></span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">appendChild</span><span class="keyword">(</span><span class="default">$bar</span><span class="keyword">);<br>print </span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">saveXML</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>This produces:<br><br>&lt;?xml version=&quot;1.0&quot;?&gt;<br>&lt;foobar&gt;&lt;foo&gt;&lt;bar/&gt;&lt;/foo&gt;&lt;/foobar&gt;<br><br>Note that the nodes &quot;&lt;foo/&gt;&quot; and &quot;&lt;bar/&gt;&quot; were siblings, i.e. the first and last child of &quot;&lt;foobar&gt;&quot; but using appendChild() we were able to move &quot;&lt;bar/&gt;&quot; so that it is a child of &quot;&lt;foo/&gt;&quot;.<br><br>This saves you the trouble of doing a DOMNode::removeChild($bar) to remove &quot;&lt;bar/&gt;&quot; before appending it as a child of &quot;&lt;foo/&gt;&quot;. <br><br>Kris Dover</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domnode.appendchild.php)

**[To root](/README.md)**