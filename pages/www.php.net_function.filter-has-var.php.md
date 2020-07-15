# filter_has_var



Please note that the function does not check the live array, it actually checks the content received by php:<br><br>

```
<?php
$_GET['test'] = 1;
echo filter_has_var(INPUT_GET, 'test') ? 'Yes' : 'No';
?>
```
<br><br>would say "No", unless the parameter was actually in the querystring.<br><br>Also, if the input var is empty, it will say Yes.  

#

Through this example i think you can better understand<br><br>    if ( !filter_has_var(INPUT_GET, &apos;email&apos;) ) {<br>        echo "Email Not Found";<br>    }else{<br>        echo "Email Found";<br>    }<br>    Output<br><br>    localhost/nanhe/test.php?email=1 //Email Found<br>    localhost/nanhe/test.php?email //Email Found<br>    http://localhost/nanhe/test.php //Email Not Found<br><br>Consider on second example<br><br>http://localhost/nanhe/test.php<br>$_GET[&apos;email&apos;]="info@nanhe.in";<br>if ( !filter_has_var(INPUT_GET, &apos;email&apos;) ) {<br>        echo "Email Not Found";<br>    }else{<br>        echo "Email Found";<br>    }<br>But output will be Email Not Found  

#

[Official documentation page](https://www.php.net/manual/en/function.filter-has-var.php)

**[To root](/README.md)**