# The SplDoublyLinkedList class




<div class="phpcode"><span class="html">
FIFO and LIFO in SplDoublyLinkedList<br><br>$list = new SplDoublyLinkedList();<br>$list-&gt;push(&apos;a&apos;);<br>$list-&gt;push(&apos;b&apos;);<br>$list-&gt;push(&apos;c&apos;);<br>$list-&gt;push(&apos;d&apos;);<br> <br>echo &quot;FIFO (First In First Out) :\n&quot;;<br>$list-&gt;setIteratorMode(SplDoublyLinkedList::IT_MODE_FIFO);<br>for ($list-&gt;rewind(); $list-&gt;valid(); $list-&gt;next()) {<br>&#xA0; &#xA0; echo $list-&gt;current().&quot;\n&quot;;<br>}<br> <br>Result :<br><br>// FIFO (First In First Out):<br>// a<br>// b<br>// c<br>// d<br> <br>echo &quot;LIFO (Last In First Out) :\n&quot;;<br>$list-&gt;setIteratorMode(SplDoublyLinkedList::IT_MODE_LIFO);<br>for ($list-&gt;rewind(); $list-&gt;valid(); $list-&gt;next()) {<br>&#xA0; &#xA0; echo $list-&gt;current().&quot;\n&quot;;<br>}<br> <br>Result :<br><br>// LIFO (Last In First Out):<br>// d<br>// c<br>// b<br>// a</span>
</div>
  

#


<div class="phpcode"><span class="html">
/*<br>php doubly link list is an amazing data structure ,doubly means you can traverse forward as well as backward, it can act as a deque(double ended queue) if you want it to,<br>here is how it works <br><br>*/<br><br>//instantiating an object of doubly link list<br><br>$dlist=new SplDoublyLinkedList();<br><br>//a push inserts data at the end of the list <br>$dlist-&gt;push(&apos;hiramariam&apos;);<br>$dlist-&gt;push(&apos;maaz&apos;);<br>$dlist-&gt;push(&apos;zafar&apos;);<br><br>/* the list contains<br>hiramariam<br>maaz<br>zafar<br>*/ <br><br>//while an unshift inserts an object at top of the list<br>$dlist-&gt;unshift(1);<br>$dlist-&gt;unshift(2);<br>$dlist-&gt;unshift(3);<br><br>/* the list now contains<br>3<br>2<br>1<br>hiramariam<br>maaz<br>zafar<br>*/ <br><br>//you can delete an item from the bottom of the list by using pop<br>$dlist-&gt;pop();<br><br>/* the list now contains<br>3<br>2<br>1<br>hiramariam<br>maaz<br><br>*/ <br>//you can delete an item from the top of the list by using shift()<br>$dlist-&gt;shift();<br><br>/* the list now contains<br><br>2<br>1<br>hiramariam<br>maaz<br><br>*/ <br><br>/* if you want to replace an item at particular index you can use a method named add , note that if you want to replace an item that does not exist , an exception will be thrown*/<br><br>$dlist-&gt;add(3 , 2.24);<br><br>/*<br>to go through the list we use a simple for loop, the rewind() method shown below point to the initials of the list depending on the iterator, a valid() method checks whether a list is still valid or not , meaning it ensures the loop does not go on and on after we reach the last data in the list , and the next() method simply points to the next data in the list.<br><br>*/<br>for($dlist-&gt;rewind();$dlist-&gt;valid();$dlist-&gt;next()){<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; echo $dlist-&gt;current().&quot;&lt;br/&gt;&quot;;<br>&#xA0; &#xA0; }<br>echo &quot;&lt;br/&gt;&quot;;<br>/*<br><br>To traverse backward <br><br>*/<br>$dlist-&gt;setIteratorMode(SplDoublyLinkedList::IT_MODE_LIFO);<br>for($dlist-&gt;rewind();$dlist-&gt;valid();$dlist-&gt;next()){<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; echo $dlist-&gt;current().&quot;&lt;br/&gt;&quot;;;<br>&#xA0; &#xA0; }</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.spldoublylinkedlist.php)

**[To root](/README.md)**