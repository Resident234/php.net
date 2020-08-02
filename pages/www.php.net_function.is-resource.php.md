# is_resource



I was recently trying to loop through some objects and convert them to arrays so that I could encode them to json strings.<br><br>I was running into issues when an element of one of my objects was a SoapClient. As it turns out, json_encode() doesn&apos;t like any resources to be passed to it. My simple fix was to use is_resource() to determine whether or not the variable I was looking at was a resource.<br><br>I quickly realized that is_resource() returns false for two out of the 3 resources that are typically in a SoapClient object. If the resource type is &apos;Unknown&apos; according to var_dump() and get_resource_type(), is_resource() doesn&apos;t think that the variable is a resource!<br><br>My work around for this was to use get_resource_type() instead of is_resource(), but that function throws an error if the variable you&apos;re checking isn&apos;t a resource.<br><br>So how are you supposed to know when a variable is a resource if is_resource() is unreliable, and get_resource_type() gives errors if you don&apos;t pass it a resource?<br><br>I ended up doing something like this:<br><br>

```
<?php

function isResource ($possibleResource) { return !is_null(@get_resource_type($possibleResource)); }

?>
```
<br><br>The @ operator suppresses the errors thrown by get_resource_type() so it returns null if $possibleResource isn&apos;t a resource.<br><br>I spent way too long trying to figure this stuff out, so I hope this comment helps someone out if they run into the same problem I did.  

---

[Official documentation page](https://www.php.net/manual/en/function.is-resource.php)

**[To root](/README.md)**