# The 




<div class="phpcode"><span class="html">
While you cannot implement this interface, you can use it in your checks to determine if something is usable in for each. Here is what I use if I&apos;m expecting something that must be iterable via foreach.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if( !</span><span class="default">is_array</span><span class="keyword">( </span><span class="default">$items </span><span class="keyword">) &amp;&amp; !</span><span class="default">$items </span><span class="keyword">instanceof </span><span class="default">Traversable </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Throw exception here<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
NOTE:&#xA0; While objects and arrays can be traversed by foreach, they do NOT implement &quot;Traversable&quot;, so you CANNOT check for foreach compatibility using an instanceof check.<br><br>Example:<br><br>$myarray = array(&apos;one&apos;, &apos;two&apos;, &apos;three&apos;);<br>$myobj = (object)$myarray;<br><br>if ( !($myarray instanceof \Traversable) ) {<br>&#xA0; &#xA0; print &quot;myarray is NOT Traversable&quot;;<br>}<br>if ( !($myobj instanceof \Traversable) ) {<br>&#xA0; &#xA0; print &quot;myobj is NOT Traversable&quot;;<br>}<br><br>foreach ($myarray as $value) {<br>&#xA0; &#xA0; print $value;<br>}<br>foreach ($myobj as $value) {<br>&#xA0; &#xA0; print $value;<br>}<br><br>Output:<br>myarray is NOT Traversable<br>myobj is NOT Traversable<br>one<br>two<br>three<br>one<br>two<br>three</span>
</div>
  

#


<div class="phpcode"><span class="html">
The PHP7 iterable pseudo type will match both Traversable and array. Great for return type-hinting so that you do not have to expose your Domain to Infrastructure code, e.g. instead of a Repository returning a Cursor, it can return hint &apos;iterable&apos;:<br><span class="default">&lt;?php<br>UserRepository</span><span class="keyword">::</span><span class="default">findUsers</span><span class="keyword">(): </span><span class="default">iterable<br>?&gt;<br></span><br>Link: <a href="http://php.net/manual/en/migration71.new-features.php#migration71.new-features.iterable-pseudo-type" rel="nofollow" target="_blank">http://php.net/manual/en/migration71.new-features.php#migration71.new-features.iterable-pseudo-type</a><br><br>Also, instead of:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if( !</span><span class="default">is_array</span><span class="keyword">( </span><span class="default">$items </span><span class="keyword">) &amp;&amp; !</span><span class="default">$items </span><span class="keyword">instanceof </span><span class="default">Traversable </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Throw exception here<br></span><span class="default">?&gt;<br></span><br>You can now do with the is_iterable() method:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if ( !</span><span class="default">is_iterable</span><span class="keyword">( </span><span class="default">$items </span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Throw exception here<br></span><span class="default">?&gt;<br></span><br>Link:&#xA0; <a href="http://php.net/manual/en/function.is-iterable.php" rel="nofollow" target="_blank">http://php.net/manual/en/function.is-iterable.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that all objects can be iterated over with foreach anyway and it&apos;ll go over each property. This just describes whether or not the class implements an iterator, i.e. has custom behaviour.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.traversable.php)

**[⬆ to root](/)**