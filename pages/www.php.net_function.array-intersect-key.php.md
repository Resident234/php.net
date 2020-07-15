# array_intersect_key



Simple key white-list filter:<br><br>

```
<?php
$arr = array('a' => 123, 'b' => 213, 'c' => 321);
$allowed = array('b', 'c');

print_r(array_intersect_key($arr, array_flip($allowed)));
?>
```
<br><br>Will return:<br>Array<br>(<br>    [b] =&gt; 213<br>    [c] =&gt; 321<br>)  

#

[Editor&apos;s note: changed array_merge_recursive() to array_replace_recursive() to fix the script]<br><br>Here is a better way to merge settings using some defaults as a whitelist.<br><br>

```
<?php

$defaults = [
    'id'            => 123456,
    'client_id'     => null,
    'client_secret' => null,
    'options'       => [
        'trusted' => false,
        'active'  => false
    ]
];

$options = [
    'client_id'       => 789,
    'client_secret'   => '5ebe2294ecd0e0f08eab7690d2a6ee69',
    'client_password' => '5f4dcc3b5aa765d61d8327deb882cf99', // ignored
    'client_name'     => 'IGNORED',                          // ignored
    'options'         => [
        'active' => true
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