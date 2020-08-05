# The SessionHandler class



As the life-cycle of a session handler is fairly complex, I found it difficult to understand when explained using just words - so I traced the function calls made to a custom SessionHandler, and created this overview of precisely what happens when you call various session methods:<br><br>https://gist.github.com/mindplay-dk/623bdd50c1b4c0553cd3<br><br>I hope this makes it considerably easier to implement a custom SessionHandler and get it right the first time :-)  

---

[Official documentation page](https://www.php.net/manual/en/class.sessionhandler.php)

**[To root](/README.md)**