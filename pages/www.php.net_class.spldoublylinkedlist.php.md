# The SplDoublyLinkedList class



FIFO and LIFO in SplDoublyLinkedList<br><br>$list = new SplDoublyLinkedList();<br>$list-&gt;push(&apos;a&apos;);<br>$list-&gt;push(&apos;b&apos;);<br>$list-&gt;push(&apos;c&apos;);<br>$list-&gt;push(&apos;d&apos;);<br> <br>echo "FIFO (First In First Out) :\n";<br>$list-&gt;setIteratorMode(SplDoublyLinkedList::IT_MODE_FIFO);<br>for ($list-&gt;rewind(); $list-&gt;valid(); $list-&gt;next()) {<br>    echo $list-&gt;current()."\n";<br>}<br> <br>Result :<br><br>// FIFO (First In First Out):<br>// a<br>// b<br>// c<br>// d<br> <br>echo "LIFO (Last In First Out) :\n";<br>$list-&gt;setIteratorMode(SplDoublyLinkedList::IT_MODE_LIFO);<br>for ($list-&gt;rewind(); $list-&gt;valid(); $list-&gt;next()) {<br>    echo $list-&gt;current()."\n";<br>}<br> <br>Result :<br><br>// LIFO (Last In First Out):<br>// d<br>// c<br>// b<br>// a  

#

/*<br>php doubly link list is an amazing data structure ,doubly means you can traverse forward as well as backward, it can act as a deque(double ended queue) if you want it to,
here is how it works 

*/

//instantiating an object of doubly link list

$dlist=new SplDoublyLinkedList();

//a push inserts data at the end of the list 
$dlist-&gt;push(&apos;hiramariam&apos;);
$dlist-&gt;push(&apos;maaz&apos;);
$dlist-&gt;push(&apos;zafar&apos;);

/* the list contains
hiramariam
maaz
zafar
*/ 

//while an unshift inserts an object at top of the list
$dlist-&gt;unshift(1);
$dlist-&gt;unshift(2);
$dlist-&gt;unshift(3);

/* the list now contains
3
2
1
hiramariam
maaz
zafar
*/ 

//you can delete an item from the bottom of the list by using pop
$dlist-&gt;pop();

/* the list now contains
3
2
1
hiramariam
maaz

*/ 
//you can delete an item from the top of the list by using shift()
$dlist-&gt;shift();

/* the list now contains

2
1
hiramariam
maaz

*/ 

/* if you want to replace an item at particular index you can use a method named add , note that if you want to replace an item that does not exist , an exception will be thrown*/

$dlist-&gt;add(3 , 2.24);

/*
to go through the list we use a simple for loop, the rewind() method shown below point to the initials of the list depending on the iterator, a valid() method checks whether a list is still valid or not , meaning it ensures the loop does not go on and on after we reach the last data in the list , and the next() method simply points to the next data in the list.

*/
for($dlist-&gt;rewind();$dlist-&gt;valid();$dlist-&gt;next()){
    
    echo $dlist-&gt;current()."&lt;br/&gt;";
    }
echo "&lt;br/&gt;";
/*

To traverse backward 

*/
$dlist-&gt;setIteratorMode(SplDoublyLinkedList::IT_MODE_LIFO);
for($dlist-&gt;rewind();$dlist-&gt;valid();$dlist-&gt;next()){
    
    echo $dlist-&gt;current()."&lt;br/&gt;";;
    }?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.spldoublylinkedlist.php)

**[To root](/README.md)**