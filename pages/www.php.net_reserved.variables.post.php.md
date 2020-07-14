# $_POST



One feature of PHP&apos;s processing of POST and GET variables is that it automatically decodes indexed form variable names.<br><br>I&apos;ve seem innumerable projects that jump through extra &amp; un-needed processing hoops to decode variables when PHP does it all for you:<br><br>Example pseudo code:<br><br>Many web sites do this:<br><br>&lt;form ....&gt;<br>&lt;input name="person_0_first_name" value="john" /&gt;<br>&lt;input name="person_0_last_name" value="smith" /&gt;<br>...<br><br>&lt;input name="person_1_first_name" value="jane" /&gt;<br>&lt;input name="person_1_last_name" value="jones" /&gt;<br>&lt;/form&gt;<br><br>When they could do this:<br><br>&lt;form ....&gt;<br>&lt;input name="person[0][first_name]" value="john" /&gt;<br>&lt;input name="person[0][last_name]" value="smith" /&gt;<br>...<br>&lt;input name="person[1][first_name]" value="jane" /&gt;<br>&lt;input name="person[1][last_name]" value="jones" /&gt;<br>&lt;/form&gt;<br><br>With the first example you&apos;d have to do string parsing / regexes to get the correct values out so they can be married with other data in your app... whereas with the second example.. you will end up with something like:<br>

```
<?php
var_dump($_POST['person']);
//will get you something like:
array (
0 => array('first_name'=>'john','last_name'=>'smith'),
1 => array('first_name'=>'jane','last_name'=>'jones'),
)
?>
```
<br><br>This is invaluable when you want to link various posted form data to other hashes on the server side, when you need to store posted data in separate "compartment" arrays or when you want to link your POSTed data into different record handlers in various Frameworks.<br><br>Remember also that using [] as in index will cause a sequential numeric array to be created once the data is posted, so sometimes it&apos;s better to define your indexes explicitly.  

#

I know it&apos;s a pretty basic thing but I had issues trying to access the $_POST variable on a form submission from my HTML page. It took me ages to work out and I couldn&apos;t find the help I needed in google. Hence this post.<br><br>Make sure your input items have the NAME attribute. The id attribute is not enough! The name attribute on your input controls is what $_POST uses to index the data and therefore show the results.  

#

There&apos;s an earlier note here about correctly referencing elements in $_POST which is accurate.  $_POST is an associative array indexed by form element NAMES, not IDs.  One way to think of it is like this:  element "id=" is for CSS, while element "name=" is for PHP.  If you are referring to your element ID in the POST array, it won&apos;t work.  You must assign a name attribute to your element to reference it correctly in the POST array.  These two attributes can be the same for simplicity, i.e., <br>&lt;input type="text" id="txtForm" name="txtForm"&gt;...&lt;/input&gt;  

#

Note that $_POST is NOT set for all HTTP POST operations,  but only for specific types of POST operations.  I have not been able to find documentation, but here&apos;s what I&apos;ve found so far.<br><br>$_POST _is_ set for:<br><br>Content-Type: application/x-www-form-urlencoded<br><br>In other words,  for standard web forms.<br><br>$_POST is NOT set for:<br><br>Content-Type:text/xml<br><br>A type used for a generic HTTP POST operation.  

#

For a page with multiple forms here is one way of processing the different POST values that you may receive.  This code is good for when you have distinct forms on a page.  Adding another form only requires an extra entry in the array and switch statements. <br><br>

```
<?php

 if (!empty($_POST))
 {
    // Array of post values for each different form on your page.
    $postNameArr = array('F1_Submit', 'F2_Submit', 'F3_Submit');        

    // Find all of the post identifiers within $_POST
    $postIdentifierArr = array();
        
    foreach ($postNameArr as $postName)
    {
        if (array_key_exists($postName, $_POST))
        {
             $postIdentifierArr[] = $postName;
        }
    }

    // Only one form should be submitted at a time so we should have one
    // post identifier.  The die statements here are pretty harsh you may consider
    // a warning rather than this. 
    if (count($postIdentifierArr) != 1)
    {
        count($postIdentifierArr) &lt; 1 or
            die("\$_POST contained more than one post identifier: " .
               implode(" ", $postIdentifierArr));

        // We have not died yet so we must have less than one.
        die("\$_POST did not contain a known post identifier.");
    }
         
    switch ($postIdentifierArr[0])
    {
    case 'F1_Submit':
       echo "Perform actual code for F1_Submit.";
       break;

    case 'Modify':
       echo "Perform actual code for F2_Submit.";
       break;
           
    case 'Delete':
       echo "Perform actual code for F3_Submit.";
       break;
    }
}
else // $_POST is empty.
{
    echo "Perform code for page without POST data. ";
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.post.php)

**[To root](/README.md)**