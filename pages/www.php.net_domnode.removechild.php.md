# DOMNode::removeChild




<div class="phpcode"><span class="html">
You can&apos;t remove DOMNodes from a DOMNodeList as you&apos;re iterating over them in a foreach loop. For example...
<br>
<br><span class="default">&lt;?php
<br>$domNodeList </span><span class="keyword">= </span><span class="default">$domDocument</span><span class="keyword">-&gt;</span><span class="default">getElementsByTagname</span><span class="keyword">(</span><span class="string">&apos;p&apos;</span><span class="keyword">);
<br>foreach ( </span><span class="default">$domNodeList </span><span class="keyword">as </span><span class="default">$domElement </span><span class="keyword">) {
<br>&#xA0; </span><span class="comment">//&#xA0; ...do stuff with $domElement...
<br>&#xA0; </span><span class="default">$domElement</span><span class="keyword">-&gt;</span><span class="default">parentNode</span><span class="keyword">-&gt;</span><span class="default">removeChild</span><span class="keyword">(</span><span class="default">$domElement</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>... will seemingly leave the internal iterator on the foreach out of wack and results will be quite strange. Though, making a queue of items to remove seems to work. For example...
<br>
<br><span class="default">&lt;?php
<br>$domNodeList </span><span class="keyword">= </span><span class="default">$domDocument</span><span class="keyword">-&gt;</span><span class="default">getElementsByTagname</span><span class="keyword">(</span><span class="string">&apos;p&apos;</span><span class="keyword">);
<br></span><span class="default">$domElemsToRemove </span><span class="keyword">= array();
<br>foreach ( </span><span class="default">$domNodeList </span><span class="keyword">as </span><span class="default">$domElement </span><span class="keyword">) {
<br>&#xA0; </span><span class="comment">// ...do stuff with $domElement...
<br>&#xA0; </span><span class="default">$domElemsToRemove</span><span class="keyword">[] = </span><span class="default">$domElement</span><span class="keyword">;
<br>}
<br>foreach( </span><span class="default">$domElemsToRemove </span><span class="keyword">as </span><span class="default">$domElement </span><span class="keyword">){
<br>&#xA0; </span><span class="default">$domElement</span><span class="keyword">-&gt;</span><span class="default">parentNode</span><span class="keyword">-&gt;</span><span class="default">removeChild</span><span class="keyword">(</span><span class="default">$domElement</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
These two functions might be helpful to anyone trying to delete a node and all of its children:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">deleteNode</span><span class="keyword">(</span><span class="default">$node</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">deleteChildren</span><span class="keyword">(</span><span class="default">$node</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$parent </span><span class="keyword">= </span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">parentNode</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$oldnode </span><span class="keyword">= </span><span class="default">$parent</span><span class="keyword">-&gt;</span><span class="default">removeChild</span><span class="keyword">(</span><span class="default">$node</span><span class="keyword">);
<br>}
<br>
<br>function </span><span class="default">deleteChildren</span><span class="keyword">(</span><span class="default">$node</span><span class="keyword">) {
<br>&#xA0; &#xA0; while (isset(</span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">firstChild</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">deleteChildren</span><span class="keyword">(</span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">firstChild</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">removeChild</span><span class="keyword">(</span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">firstChild</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domnode.removechild.php)

**[To root](/README.md)**