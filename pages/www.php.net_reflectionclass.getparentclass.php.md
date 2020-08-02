# ReflectionClass::getParentClass



Quick correction, the code for getting all parent classes below has a "typo", you need to reset the $class variable to the parent class instance, otherwise it just endlessly loops:<br><br>        $class = new ReflectionClass(&apos;classname&apos;);<br>        <br>        $parents = array();<br>        <br>        while ($parent = $class-&gt;getParentClass()) {<br>            $parents[] = $parent-&gt;getName();<br>            $class = $parent;<br>        }<br>        <br>        echo "Parents: " . implode(", ", $parents);  

---

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getparentclass.php)

**[To root](/README.md)**