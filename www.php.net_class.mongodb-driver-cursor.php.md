# The MongoDB\Driver\Cursor class




<div class="phpcode"><span class="html">
As one might notice, this class does not implement a hasNext() or next() method as opposed to the now deprecated Mongo driver.<br><br>If, for any reason, you need to pull data from the cursor procedurally or otherwise need to override the behavior of foreach while iterating on the cursor, the SPL \IteratorIterator class can be used. When doing so, it is important to rewind the iterator before using it, otherwise you won&apos;t get any data back.<br><br><span class="default">&lt;?php<br>$cursor </span><span class="keyword">= </span><span class="default">$collection</span><span class="keyword">-&gt;</span><span class="default">find</span><span class="keyword">();<br></span><span class="default">$it </span><span class="keyword">= new \</span><span class="default">IteratorIterator</span><span class="keyword">(</span><span class="default">$cursor</span><span class="keyword">);<br></span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">rewind</span><span class="keyword">(); </span><span class="comment">// Very important<br><br></span><span class="keyword">while(</span><span class="default">$doc </span><span class="keyword">= </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">current</span><span class="keyword">()) {<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$doc</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$it</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">();<br>}<br></span><span class="default">?&gt;<br></span><br>I used this trick to build a backward compatibility wrapper emulating the old Mongo driver in order to upgrade an older codebase.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-cursor.php)

**[⬆ to root](/)**