# The SplQueue class




<div class="phpcode"><span class="html">
SplQueue inherits from SplDoublyLinkedList. So, objects of SplQueue support methods push() and pop(). But please be advised that if you use push() and pop() methods on a SplQueue object, it behaves like a stack rather than a queue.<br><br>For example:<br><br>$q = new SplQueue();<br>$q-&gt;push(1);<br>$q-&gt;push(2);<br>$q-&gt;push(3);<br>$q-&gt;pop();<br>print_r($q);<br><br>Above code returns:<br><br>SplQueue Object<br>(<br>&#xA0; &#xA0; [flags:SplDoublyLinkedList:private] =&gt; 4<br>&#xA0; &#xA0; [dllist:SplDoublyLinkedList:private] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>)<br><br>Note that 3 got popped and *not* 1.<br><br>So, please make sure that you use only enqueue() and dequeue() methods on a SplQueue object and *not* push() and pop().</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.splqueue.php)

**[⬆ to root](/)**