# Escaping from HTML





When the documentation says that the PHP parser ignores everything outside the 

```
<?php ... ?>
```
 tags, it means literally EVERYTHING. Including things you normally wouldn&apos;t consider &quot;valid&quot;, such as the following:

&lt;html&gt;&lt;body&gt;
&lt;p

```
<?php if ($highlight): ?>
```
 class=&quot;highlight&quot;

```
<?php endif;?>
```
&gt;This is a paragraph.&lt;/p&gt;
&lt;/body&gt;&lt;/html&gt;

Notice how the PHP code is embedded in the middle of an HTML opening tag. The PHP parser doesn&apos;t care that it&apos;s in the middle of an opening tag, and doesn&apos;t require that it be closed. It also doesn&apos;t care that after the closing ?>
```
 tag is the end of the HTML opening tag. So, if $highlight is true, then the output will be:

&lt;html&gt;&lt;body&gt;
&lt;p class=&quot;highlight&quot;&gt;This is a paragraph.&lt;/p&gt;
&lt;/body&gt;&lt;/html&gt;

Otherwise, it will be:

&lt;html&gt;&lt;body&gt;
&lt;p&gt;This is a paragraph.&lt;/p&gt;
&lt;/body&gt;&lt;/html&gt;

Using this method, you can have HTML tags with optional attributes, depending on some PHP condition. Extremely flexible and useful!

  

#



One aspect of PHP that you need to be careful of, is that ?>
```
 will drop you out of PHP code and into HTML even if it appears inside a // comment. (This does not apply to /* */ comments.) This can lead to unexpected results. For example, take this line:



```
<?php
&#xA0; $file_contents&#xA0; = &apos;

```
<?php die(); ?>
```
&apos; . &quot;\n&quot;;
?>
```


If you try to remove it by turning it into a comment, you get this:



```
<?php
//&#xA0; $file_contents&#xA0; = &apos;

```
<?php die(); ?>
```
&apos; . &quot;\n&quot;;
?>
```


Which results in &apos; . &quot;\n&quot;; (and whatever is in the lines following it) to be output to your HTML page.

The cure is to either comment it out using /* */ tags, or re-write the line as:



```
<?php
&#xA0; $file_contents&#xA0; = &apos;&lt;&apos; . &apos;?php die(); ?&apos; . &apos;&gt;&apos; . &quot;\n&quot;;
?>
```



  

#



Although not specifically pointed out in the main text, escaping from HTML also applies to other control statements:



```
<?php for ($i = 0; $i &lt; 5; ++$i): ?>
```

Hello, there!


```
<?php endfor; ?>
```


When the above code snippet is executed we get the following output:

Hello, there!
Hello, there!
Hello, there!
Hello, there!

  

#



Playing around with different open and close tags I discovered you can actually mix different style open/close tags

some examples

&lt;%
//your php code here
?>
```


or

&lt;script language=&quot;php&quot;&gt;
//php code here
%&gt;

  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.phpmode.php)

**[To root](/README.md)**