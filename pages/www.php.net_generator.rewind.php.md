# Generator::rewind



Actually, this method can be useful to test a generator before iterating, as it executes your function up to the first yield statement. I.e. if you try to read a non-existent file in a generator, an error will normally occur only in client code foreach()&apos;s first iteration. Sometimes this can be critical to check beforehand.<br><br>Take a look at a modified example from here:<br>http://php.net/manual/ru/language.generators.overview.php#112985<br><br>

```
<?php

function getLines($file) {
    $f = fopen($file, &apos;r&apos;);
    try {
        while ($line = fgets($f)) {
            yield $line;
        }
    } finally {
        fclose($f);
    }
}

$getLines = getLines(&apos;no_such_file.txt&apos;);
$getLines-&gt;rewind(); // with -&gt;rewind(), a file read error will be thrown here and a log file will not be cleared

openAndClearLogFile();

foreach ($getLines as $n =&gt; $line) { // without -&gt;rewind(), the script will die here and your log file will be cleared
    writeToLogFile(&apos;reading: &apos; . $line . "\n");
}

closeLogFile();

?>
```
<br><br>P.S.: When you iterate over a generator after -&gt;rewind(), you&apos;ll get the first yielded value immediately, as the preceding code was already executed.  

#

[Official documentation page](https://www.php.net/manual/en/generator.rewind.php)

**[To root](/README.md)**