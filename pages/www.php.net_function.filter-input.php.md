# filter_input



This function provides us the extremely simple solution for type filtering.<br><br>Without this function...<br>

```
<?php
if (!isset($_GET[&apos;a&apos;])) {
    $a = null;
} elseif (!is_string($_GET[&apos;a&apos;])) {
    $a = false;
} else {
    $a = $_GET[&apos;a&apos;];
}
$b = isset($_GET[&apos;b&apos;]) &amp;&amp; is_string($_GET[&apos;b&apos;]) ? $_GET[&apos;b&apos;] : &apos;&apos;;
?>
```


With this function...


```
<?php
$a = filter_input(INPUT_GET, &apos;a&apos;);
$b = (string)filter_input(INPUT_GET, &apos;b&apos;);
?>
```
<br><br>Yes, FILTER_REQUIRE_SCALAR seems to be set as a default option. <br>It&apos;s very helpful for eliminating E_NOTICE, E_WARNING and E_ERROR. <br>This fact should be documented.  

#

FastCGI seems to cause strange side-effects with unexpected null values when using INPUT_SERVER and INPUT_ENV with this function. You can use this code to see if it affects your server:<br>

```
<?php
var_dump($_SERVER);
foreach ( array_keys($_SERVER) as $b ) {
    var_dump($b, filter_input(INPUT_SERVER, $b));
}
echo &apos;&lt;hr&gt;&apos;;
var_dump($_ENV);
foreach ( array_keys($_ENV) as $b ) {
    var_dump($b, filter_input(INPUT_ENV, $b));
}
?>
```
<br>If you want to be on the safe side, using the superglobal $_SERVER and $_ENV variables will always work. You can still use the filter_* functions for Get/Post/Cookie without a problem, which is the important part!  

#

If your $_POST contains an array value:<br>

```
<?php
$_POST  = array(
    &apos;var&apos; =&gt; array(&apos;more&apos;, &apos;than&apos;, &apos;one&apos;, &apos;values&apos;)
);
?>
```

you should use FILTER_REQUIRE_ARRAY option:


```
<?php
var_dump(filter_input(INPUT_POST, &apos;var&apos;, FILTER_DEFAULT , FILTER_REQUIRE_ARRAY));
?>
```
<br>Otherwise it returns false.  

#

Note that this function doesn&apos;t (or at least doesn&apos;t seem to) actually filter based on the current values of $_GET etc. Instead, it seems to filter based off the original values.<br>

```
<?php
$_GET[&apos;search&apos;] = &apos;foo&apos;; // This has no effect on the filter_input

$search_html = filter_input(INPUT_GET, &apos;search&apos;, FILTER_SANITIZE_SPECIAL_CHARS);
$search_url = filter_input(INPUT_GET, &apos;search&apos;, FILTER_SANITIZE_ENCODED);
echo "You have searched for $search_html.\n";
echo "&lt;a href=&apos;?search=$search_url&apos;&gt;Search again.&lt;/a&gt;";
?>
```
<br><br>If you need to set a default input value and filter that, use filter_var on your required input variable instead  

#

To use a class method for a callback function, as usual, provide an array with an instance of the class and the method name.<br>Example:<br><br>

```
<?php
class myValidator
{
  public function username($value)
  {
    // return username or boolean false
  }
}

$myValidator = new myValidator;
$options = array(&apos;options&apos; =&gt; array($myValidator, &apos;username&apos;));
$username = filter_input(INPUT_GET, &apos;username&apos;, FILTER_CALLBACK, $options);
var_dump($username);
?>
```
  

#

Here is an example how to work with the options-parameter. Notice the &apos;options&apos; in the &apos;options&apos;-Parameter!<br><br>

```
<?php
$options=array(&apos;options&apos;=&gt;array(&apos;default&apos;=&gt;5, &apos;min_range&apos;=&gt;0, &apos;max_range&apos;=&gt;9));

$priority=filter_input(INPUT_GET, &apos;priority&apos;, FILTER_VALIDATE_INT, $options);
?>
```
<br><br>$priority will be 5 if the priority-Parameter isn&apos;t set or out the given range.  

#

[Official documentation page](https://www.php.net/manual/en/function.filter-input.php)

**[To root](/README.md)**