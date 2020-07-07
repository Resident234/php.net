# $_POST





One feature of PHP&apos;s processing of POST and GET variables is that it automatically decodes indexed form variable names.

I&apos;ve seem innumerable projects that jump through extra &amp; un-needed processing hoops to decode variables when PHP does it all for you:

Example pseudo code:

Many web sites do this:

&lt;form ....&gt;
&lt;input name=&quot;person_0_first_name&quot; value=&quot;john&quot; /&gt;
&lt;input name=&quot;person_0_last_name&quot; value=&quot;smith&quot; /&gt;
...

&lt;input name=&quot;person_1_first_name&quot; value=&quot;jane&quot; /&gt;
&lt;input name=&quot;person_1_last_name&quot; value=&quot;jones&quot; /&gt;
&lt;/form&gt;

When they could do this:

&lt;form ....&gt;
&lt;input name=&quot;person[0][first_name]&quot; value=&quot;john&quot; /&gt;
&lt;input name=&quot;person[0][last_name]&quot; value=&quot;smith&quot; /&gt;
...
&lt;input name=&quot;person[1][first_name]&quot; value=&quot;jane&quot; /&gt;
&lt;input name=&quot;person[1][last_name]&quot; value=&quot;jones&quot; /&gt;
&lt;/form&gt;

With the first example you&apos;d have to do string parsing / regexes to get the correct values out so they can be married with other data in your app... whereas with the second example.. you will end up with something like:


```
<?php
var_dump($_POST[&apos;person&apos;]);
//will get you something like:
array (
0 =&gt; array(&apos;first_name&apos;=&gt;&apos;john&apos;,&apos;last_name&apos;=&gt;&apos;smith&apos;),
1 =&gt; array(&apos;first_name&apos;=&gt;&apos;jane&apos;,&apos;last_name&apos;=&gt;&apos;jones&apos;),
)
?>
```


This is invaluable when you want to link various posted form data to other hashes on the server side, when you need to store posted data in separate &quot;compartment&quot; arrays or when you want to link your POSTed data into different record handlers in various Frameworks.

Remember also that using [] as in index will cause a sequential numeric array to be created once the data is posted, so sometimes it&apos;s better to define your indexes explicitly.

  

#



I know it&apos;s a pretty basic thing but I had issues trying to access the $_POST variable on a form submission from my HTML page. It took me ages to work out and I couldn&apos;t find the help I needed in google. Hence this post.

Make sure your input items have the NAME attribute. The id attribute is not enough! The name attribute on your input controls is what $_POST uses to index the data and therefore show the results.

  

#



There&apos;s an earlier note here about correctly referencing elements in $_POST which is accurate.&#xA0; $_POST is an associative array indexed by form element NAMES, not IDs.&#xA0; One way to think of it is like this:&#xA0; element &quot;id=&quot; is for CSS, while element &quot;name=&quot; is for PHP.&#xA0; If you are referring to your element ID in the POST array, it won&apos;t work.&#xA0; You must assign a name attribute to your element to reference it correctly in the POST array.&#xA0; These two attributes can be the same for simplicity, i.e., 
&lt;input type=&quot;text&quot; id=&quot;txtForm&quot; name=&quot;txtForm&quot;&gt;...&lt;/input&gt;

  

#



Note that $_POST is NOT set for all HTTP POST operations,&#xA0; but only for specific types of POST operations.&#xA0; I have not been able to find documentation, but here&apos;s what I&apos;ve found so far.

$_POST _is_ set for:

Content-Type: application/x-www-form-urlencoded

In other words,&#xA0; for standard web forms.

$_POST is NOT set for:

Content-Type:text/xml

A type used for a generic HTTP POST operation.

  

#



For a page with multiple forms here is one way of processing the different POST values that you may receive.&#xA0; This code is good for when you have distinct forms on a page.&#xA0; Adding another form only requires an extra entry in the array and switch statements. 



```
<?php

 if (!empty($_POST))
 {
&#xA0; &#xA0; // Array of post values for each different form on your page.
&#xA0; &#xA0; $postNameArr = array(&apos;F1_Submit&apos;, &apos;F2_Submit&apos;, &apos;F3_Submit&apos;);&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; // Find all of the post identifiers within $_POST
&#xA0; &#xA0; $postIdentifierArr = array();
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; foreach ($postNameArr as $postName)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (array_key_exists($postName, $_POST))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $postIdentifierArr[] = $postName;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; // Only one form should be submitted at a time so we should have one
&#xA0; &#xA0; // post identifier.&#xA0; The die statements here are pretty harsh you may consider
&#xA0; &#xA0; // a warning rather than this. 
&#xA0; &#xA0; if (count($postIdentifierArr) != 1)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; count($postIdentifierArr) &lt; 1 or
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die(&quot;\$_POST contained more than one post identifier: &quot; .
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; implode(&quot; &quot;, $postIdentifierArr));

&#xA0; &#xA0; &#xA0; &#xA0; // We have not died yet so we must have less than one.
&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;\$_POST did not contain a known post identifier.&quot;);
&#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; switch ($postIdentifierArr[0])
&#xA0; &#xA0; {
&#xA0; &#xA0; case &apos;F1_Submit&apos;:
&#xA0; &#xA0; &#xA0;&#xA0; echo &quot;Perform actual code for F1_Submit.&quot;;
&#xA0; &#xA0; &#xA0;&#xA0; break;

&#xA0; &#xA0; case &apos;Modify&apos;:
&#xA0; &#xA0; &#xA0;&#xA0; echo &quot;Perform actual code for F2_Submit.&quot;;
&#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; case &apos;Delete&apos;:
&#xA0; &#xA0; &#xA0;&#xA0; echo &quot;Perform actual code for F3_Submit.&quot;;
&#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; }
}
else // $_POST is empty.
{
&#xA0; &#xA0; echo &quot;Perform code for page without POST data. &quot;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.post.php)

**[To root](/README.md)**