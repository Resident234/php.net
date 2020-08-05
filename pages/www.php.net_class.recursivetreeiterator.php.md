# The RecursiveTreeIterator class



$it = new RecursiveArrayIterator(array(1, 2, array(3, 4, array(5, 6, 7), 8), 9, 10));<br>$tit = new RecursiveTreeIterator($it);<br><br>foreach( $tit as $key =&gt; $value ){<br>    echo $value . PHP_EOL;<br>}<br><br>/* Will output<br><br>|-1<br>|-2<br>|-Array<br>| |-3<br>| |-4<br>| |-Array<br>| | |-5<br>| | |-6<br>| | \-7<br>| \-8<br>|-9<br>\-10<br><br>*/  

---

[Official documentation page](https://www.php.net/manual/en/class.recursivetreeiterator.php)

**[To root](/README.md)**