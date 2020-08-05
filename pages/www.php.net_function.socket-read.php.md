# socket_read



quote:<br>"Note:<br>socket_read() returns a zero length string ("") when there is no more data to read."<br><br>This is not true!<br><br>In a while loop  <br>(example case few bytes to receive - just enough for 1 call, but you use a loop to be sure you received all data)<br>if you use <br>

```
<?php socket_set_block($socket); ?>
```

you will get:
1st call in loop: data
2nd call in loop: a block forever, if there isnt data anymore or w/e happen to the "other side"

So ofc you want to use 


```
<?php socket_set_nonblock($socket); ?>
```
<br>and you will get:<br>1st call in loop: data<br>2nd call in loop: socket_read() returns FALSE (bool) and socket_last_error() gives you a SOCKET_EWOULDBLOCK (http://de1.php.net/manual/de/sockets.constants.php)<br><br>There is not a single time i got a empty string back from socket_read.<br>And im "working" on this problem(bug?) since a week or so.<br><br>You better use socket_recv() instead.<br>(good luck)  

---

[Official documentation page](https://www.php.net/manual/en/function.socket-read.php)

**[To root](/README.md)**