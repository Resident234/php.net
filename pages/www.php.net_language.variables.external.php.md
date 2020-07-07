# Variables From External Sources





The full list of field-name characters that PHP converts to _ (underscore) is the following (not just dot):
chr(32) ( ) (space)
chr(46) (.) (dot)
chr(91) ([) (open square bracket)
chr(128) - chr(159) (various)

PHP irreversibly modifies field names containing these characters in an attempt to maintain compatibility with the deprecated register_globals feature.

  

#



Important:&#xA0; Pay attention to the following security concerns when handling user submitted&#xA0; data :



http://www.php.net/manual/en/security.registerglobals.php

http://www.php.net/manual/en/security.variables.php

  

#



This post is with regards to handling forms that have more than one submit button.



Suppose we have an HTML form with a submit button specified like this:



&lt;input type=&quot;submit&quot; value=&quot;Delete&quot; name=&quot;action_button&quot;&gt;



Normally the &apos;value&apos; attribute of the HTML &apos;input&apos; tag (in this case &quot;Delete&quot;) that creates the submit button can be accessed in PHP after post like this:





```
<?php

$_POST[&apos;action_button&apos;];

?>
```




We of course use the &apos;name&apos; of the button as an index into the $_POST array.



This works fine, except when we want to pass more information with the click of this particular button.



Imagine a scenario where you&apos;re dealing with user management in some administrative interface.&#xA0; You are presented with a list of user names queried from a database and wish to add a &quot;Delete&quot; and &quot;Modify&quot; button next to each of the names in the list.&#xA0; Naturally the &apos;value&apos; of our buttons in the HTML form that we want to display will be &quot;Delete&quot; and &quot;Modify&quot; since that&apos;s what we want to appear on the buttons&apos; faceplates.



Both buttons (Modify and Delete) will be named &quot;action_button&quot; since that&apos;s what we want to index the $_POST array with.&#xA0; In other words, the &apos;name&apos; of the buttons along cannot carry any uniquely identifying information if we want to process them systematically after submit. Since these buttons will exist for every user in the list, we need some further way to distinguish them, so that we know for which user one of the buttons has been pressed.



Using arrays is the way to go.&#xA0; Assuming that we know the unique numerical identifier of each user, such as their primary key from the database, and we DON&apos;T wish to protect that number from the public, we can make the &apos;action_button&apos; into an array and use the user&apos;s unique numerical identifier as a key in this array.



Our HTML code to display the buttons will become:



&lt;input type=&quot;submit&quot; value=&quot;Delete&quot; name=&quot;action_button[0000000002]&quot;&gt;

&lt;input type=&quot;submit&quot; value=&quot;Modify&quot; name=&quot;action_button[0000000002]&quot;&gt;



The 0000000002 is of course the unique numerical identifier for this particular user.



Then when we handle this form in PHP we need to do the following to extract both the &apos;value&apos; of the button (&quot;Delete&quot; or &quot;Modify&quot;) and the unique numerical identifier of the user we wish to affect (0000000002 in this case). The following will print either &quot;Modify&quot; or &quot;Delete&quot;, as well as the unique number of the user:





```
<?php

$submitted_array = array_keys($_POST[&apos;action_button&apos;]);

echo ($_POST[&apos;action_button&apos;][$submitted_array[0]] . &quot; &quot; . $submitted_array[0]);

?>
```




$submitted_array[0] carries the 0000000002.

When we index that into the $_POST[&apos;action_button&apos;], like we did above, we will extract the string that was used as &apos;value&apos; in the HTML code &apos;input&apos; tag that created this button.



If we wish to protect the unique numerical identifier, we must use some other uniquely identifying attribute of each user. Possibly that attribute should be encrypted when output into the form for greater security.



Enjoy!

  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.external.php)

**[To root](/README.md)**