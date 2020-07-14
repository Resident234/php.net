# array_intersect_key



Simple key white-list filter:<br><br>

```
<?php
$arr = array(&apos;a&apos; =&gt; 123, &apos;b&apos; =&gt; 213, &apos;c&apos; =&gt; 321);
$allowed = array(&apos;b&apos;, &apos;c&apos;);

print_r(array_intersect_key($arr, array_flip($allowed)));
?>
```
<br><br>Will return:<br>Array<br>(<br>    [b] =&gt; 213<br>    [c] =&gt; 321<br>)  

#

[Editor&apos;s note: changed array_merge_recursive() to array_replace_recursive() to fix the script]<br><br>Here is a better way to merge settings using some defaults as a whitelist.<br><br>

```
<?php

$defaults = [
    &apos;id&apos;            =&gt; 123456,
    &apos;client_id&apos;     =&gt; null,
    &apos;client_secret&apos; =&gt; null,
    &apos;options&apos;       =&gt; [
        &apos;trusted&apos; =&gt; false,
        &apos;active&apos;  =&gt; false
    ]
];

$options = [
    &apos;client_id&apos;       =&gt; 789,
    &apos;client_secret&apos;   =&gt; &apos;5ebe2294ecd0e0f08eab7690d2a6ee69&apos;,
    &apos;client_password&apos; =&gt; &apos;5f4dcc3b5aa765d61d8327deb882cf99&apos;, // ignored
    &apos;client_name&apos;     =&gt; &apos;IGNORED&apos;,                          // ignored
    &apos;options&apos;         =&gt; [
        &apos;active&apos; =&gt; true
    ]
];

var_dump(
    array_replace_recursive($defaults, 
        array_intersect_key(
            $options, $defaults
        )
    )
);

?>
```
<br><br>Output:<br><br>array (size=4)<br>    &apos;id&apos;            =&gt; int 123456<br>    &apos;client_id&apos;     =&gt; int 789<br>    &apos;client_secret&apos; =&gt; string &apos;5ebe2294ecd0e0f08eab7690d2a6ee69&apos; (length=32)<br>    &apos;options&apos;       =&gt; <br>        array (size=2)<br>            &apos;trusted&apos; =&gt; boolean false<br>            &apos;active&apos;  =&gt; boolean true  

#

[Official documentation page](https://www.php.net/manual/en/function.array-intersect-key.php)

**[To root](/README.md)**