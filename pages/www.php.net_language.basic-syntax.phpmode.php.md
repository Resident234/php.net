# Escaping from HTML



When the documentation says that the PHP parser ignores everything outside the 

```
<?php ... ?>
```
 tags, it means literally EVERYTHING. Including things you normally wouldn't consider "valid", such as the following:

<html><body>
<p

```
<?php if ($highlight): ?>
```
 class="highlight"

```
<?php endif;?>
```
>This is a paragraph.</p>
</body></html>

Notice how the PHP code is embedded in the middle of an HTML opening tag. The PHP parser doesn't care that it's in the middle of an opening tag, and doesn't require that it be closed. It also doesn't care that after the closing ?>
```
 tag is the end of the HTML opening tag. So, if $highlight is true, then the output will be:<br><br>&lt;html&gt;&lt;body&gt;<br>&lt;p class="highlight"&gt;This is a paragraph.&lt;/p&gt;<br>&lt;/body&gt;&lt;/html&gt;<br><br>Otherwise, it will be:<br><br>&lt;html&gt;&lt;body&gt;<br>&lt;p&gt;This is a paragraph.&lt;/p&gt;<br>&lt;/body&gt;&lt;/html&gt;<br><br>Using this method, you can have HTML tags with optional attributes, depending on some PHP condition. Extremely flexible and useful!  

---

One aspect of PHP that you need to be careful of, is that ?>
```
 will drop you out of PHP code and into HTML even if it appears inside a // comment. (This does not apply to /* */ comments.) This can lead to unexpected results. For example, take this line:<br><br>

```
<?php
  $file_contents  = '

```
<?php die(); ?>
```
' . "\n";
?>
```


If you try to remove it by turning it into a comment, you get this:



```
<?php
//  $file_contents  = '

```
<?php die(); ?>
```
' . "\n";
?>
```


Which results in ' . "\n"; (and whatever is in the lines following it) to be output to your HTML page.

The cure is to either comment it out using /* */ tags, or re-write the line as:



```
<?php
  $file_contents  = '<' . '?php die(); ?' . '>' . "\n";
?>
```
  

---

Although not specifically pointed out in the main text, escaping from HTML also applies to other control statements:<br><br>

```
<?php for ($i = 0; $i < 5; ++$i): ?>
```

Hello, there!


```
<?php endfor; ?>
```
<br><br>When the above code snippet is executed we get the following output:<br><br>Hello, there!<br>Hello, there!<br>Hello, there!<br>Hello, there!  

---

Playing around with different open and close tags I discovered you can actually mix different style open/close tags<br><br>some examples<br><br>&lt;%<br>//your php code here<br>?>
```
<br><br>or<br><br>&lt;script language="php"&gt;<br>//php code here<br>%&gt;  

---

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.phpmode.php)

**[To root](/README.md)**