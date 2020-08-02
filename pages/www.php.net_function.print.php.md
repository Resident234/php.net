# print



Be careful when using print. Since print is a language construct and not a function, the parentheses around the argument is not required.<br>In fact, using parentheses can cause confusion with the syntax of a function and SHOULD be omited.<br><br>Most would expect the following behavior:<br>

```
<?php
    if (print("foo") &amp;&amp; print("bar")) {
        // "foo" and "bar" had been printed
    }
?>
```


But since the parenthesis around the argument are not required, they are interpretet as part of the argument.
This means that the argument of the first print is

    ("foo") &amp;&amp; print("bar")

and the argument of the second print is just

    ("bar")

For the expected behavior of the first example, you need to write: 


```
<?php
    if ((print "foo") &amp;&amp; (print "bar")) {
        // "foo" and "bar" had been printed
    }
?>
```
  

---

I wrote a println function that determines whether a \n or a &lt;br /&gt; should be appended to the line depending on whether it&apos;s being executed in a shell or a browser window.  People have probably thought of this before but I thought I&apos;d post it anyway - it may help a couple of people.<br><br>

```
<?php
function println ($string_message) {
    $_SERVER['SERVER_PROTOCOL'] ? print "$string_message<br />" : print "$string_message\n";
}
?>
```


Examples:

Running in a browser:



```
<?php println ("Hello, world!"); ?>
```

Output: Hello, world!<br />

Running in a shell:



```
<?php println ("Hello, world!"); ?>
```
<br>Output: Hello, world!\n  

---

[Official documentation page](https://www.php.net/manual/en/function.print.php)

**[To root](/README.md)**