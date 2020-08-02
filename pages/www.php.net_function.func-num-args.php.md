# func_num_args



Just a note for anyone wondering. This function doesn&apos;t include params that have a default value, unless you pass one in to overwrite the default param value. Not sure if that makes sense, so here&apos;s an example:<br><br>

```
<?php
function helloWorld($ArgA, $ArgB="HelloWorld!") {
  return func_num_args();
}

// The following will return 1
$Returns1 = helloWorld("HelloWorld!");

// The following will return 2
$Returns2 = helloWorld("HelloWorld!", "HowdyWorld!");
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.func-num-args.php)

**[To root](/README.md)**