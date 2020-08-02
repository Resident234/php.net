# The SplQueue class



SplQueue inherits from SplDoublyLinkedList. So, objects of SplQueue support methods push() and pop(). But please be advised that if you use push() and pop() methods on a SplQueue object, it behaves like a stack rather than a queue.<br><br>For example:<br><br>$q = new SplQueue();<br>$q-&gt;push(1);<br>$q-&gt;push(2);<br>$q-&gt;push(3);<br>$q-&gt;pop();<br>print_r($q);<br><br>Above code returns:<br><br>SplQueue Object<br>(<br>    [flags:SplDoublyLinkedList:private] =&gt; 4<br>    [dllist:SplDoublyLinkedList:private] =&gt; Array<br>        (<br>            [0] =&gt; 1<br>            [1] =&gt; 2<br>        )<br>)<br><br>Note that 3 got popped and *not* 1.<br><br>So, please make sure that you use only enqueue() and dequeue() methods on a SplQueue object and *not* push() and pop().  

---

[Official documentation page](https://www.php.net/manual/en/class.splqueue.php)

**[To root](/README.md)**