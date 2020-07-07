# Generator::rewind





Actually, this method can be useful to test a generator before iterating, as it executes your function up to the first yield statement. I.e. if you try to read a non-existent file in a generator, an error will normally occur only in client code foreach()&apos;s first iteration. Sometimes this can be critical to check beforehand.

Take a look at a modified example from here:
http://php.net/manual/ru/language.generators.overview.php#112985



```
<?php

function getLines($file) {
&#xA0; &#xA0; $f = fopen($file, &apos;r&apos;);
&#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; while ($line = fgets($f)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; yield $line;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; } finally {
&#xA0; &#xA0; &#xA0; &#xA0; fclose($f);
&#xA0; &#xA0; }
}

$getLines = getLines(&apos;no_such_file.txt&apos;);
$getLines-&gt;rewind(); // with -&gt;rewind(), a file read error will be thrown here and a log file will not be cleared

openAndClearLogFile();

foreach ($getLines as $n =&gt; $line) { // without -&gt;rewind(), the script will die here and your log file will be cleared
&#xA0; &#xA0; writeToLogFile(&apos;reading: &apos; . $line . &quot;\n&quot;);
}

closeLogFile();

?>
```


P.S.: When you iterate over a generator after -&gt;rewind(), you&apos;ll get the first yielded value immediately, as the preceding code was already executed.

  

#

[Official documentation page](https://www.php.net/manual/en/generator.rewind.php)

**[To root](/README.md)**