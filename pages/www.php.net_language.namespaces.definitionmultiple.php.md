# Defining multiple namespaces in the same file



using of global namespaces and multiple namespaces in one PHP file increase the complexity and decrease readability of the code.<br>Let&apos;s try not use this scheme even it&apos;s very necessary (although there is not)  

#



```
<?php<br><br>// You cannot mix bracketed namespace declarations with unbracketed namespace declarations - will result in a Fatal error<br><br>namespace a;<br><br>echo "I belong to namespace a";<br><br>namespace b {<br>    echo "I&apos;m from namespace b";<br>}  

#



```
<?php<br>//Namespace can be used in this way also<br>namespace MyProject {<br><br>function connect() { echo "ONE";  }<br>    Sub\Level\connect();<br>}<br><br>namespace MyProject\Sub {<br>    <br>function connect() { echo "TWO";  }<br>    Level\connect();<br>}<br><br>namespace MyProject\Sub\Level {<br>    <br>    function connect() { echo "THREE";  }    <br>    \MyProject\Sub\Level\connect(); // OR we can use this as below<br>    connect();<br>}  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definitionmultiple.php)

**[To root](/README.md)**